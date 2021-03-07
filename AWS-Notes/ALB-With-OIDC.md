# vertx-pac4j-oidc
### Purpose
* An AWS ALB OIDC headless test client for idp integration
### Attribution
* This is based on vertx-pac4j-demo
### AWS ALB OIDC Configuration Notes
* [Okta blog entry describing workflow](https://developer.okta.com/blog/2018/01/11/sso-auth-vertx)
* [AWS Demo Page](https://exampleloadbalancer.com/auth_demo.html)
(Hopefully I transcribed this correctly)
```
User sends HTTPS request to ALB
ALB checks for AWSELBAuthSessionCookie session cookie and redirects the client (wiht a 302) to idp authorization (actually authentication) endpoint with a modified query string, if the cookie is missing.
The updated query string passed to the IDP includes
  response_type=code
  scope=<the list of scopes>
  client_id=<client id>
  redirect_uri=<the ALB callback URL>
  state=<a static random string passed around to prevent against replay of this credential negotiation>
Then IDP may redirect the user to its login or authorization endpoints with several 302's
IDP prompts the user to authenticate.
Upon successful authentication the IDP redirects the user (302) to the alb callback endpoint from the redirect_uri, and to the query string adds
  code=<the alb expects this to be an unencrypted JWT - but the IDP does not need it to be>
  state=<the state string>
  scope=<scopes>
The ALB requests a token for use with the userinfo endpoint from the token endpoint by passing the `code`.
The ALB passes the token to the userinfo endpoint.
The userinfo endpoint is OIDC aware and decrypts the token, validates the JWT if it wants to, and validates the token with the authorization endpoint.
The userinfo endpoint returns user information to the ALB appropriate to that userinfo endpoint (as an idp could have more than one userinfo endpoint for different purposes).
<ALB caches information for X-AMZN-OIDC-* headers, which can be looked up by AWSELBAuthSessionCookie?>
<ALB sets AWSELBAuthSessionCookie cookie?>
alb continues normal action (or returns a 500 if there was a problem)
User requests new uri (AWSELBAuthSessionCookie already set)
ALB validates cookie and forwards user info to backend in X-AMZN-OIDC-* headers
Backend sends response back via ALB (normal)
```

* [AWS ALB Auth Notes](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html)
### The workflow per AWS mixed with my notes
* Me: Although the ALB configuration option is OIDC (OpenID Connect), it actually uses Oauth2 to communicate with the IDP, but passes that information on in a more OIDC like format to the endpoint.
* Me: Because the ALB does not actually use OIDC it does not know about any of the keys at the well know endpoints to decrypt a JWT.
* Me: OpenID has many endpoints, including encrytion, and autodiscovery (issuer_uri/.well-know/openid-configuration) but AWS does not use them.
* AWS: Callback is `https://<alb_fqdn>/oauth2/idpresponse`
* Me: The AWS ALB cannot be configured to use an internal DNS server - for instance to resolve the hostname of an internal IDP.
* Me: The IDP hostname must be publically resolvable and accessible to the ALB (over the internet), even if the ALB is only accessable internally
* AWS: OAuth2 Grant type must be `Authorization code grant` (not Implicit)
* AWS: ALB displays 401 Unathorized when the issuer, token or authorization endpoint is not correct or the client id/secret is not correct
* AWS: You must use S256 (might also be ES256)
* Me: Code, State, Scope, Extra Params, and redirect are passed as a part of the 302/GET query string between the IDP, ALB, and the user's browser.
* Me: The ALB expects an unencrypted JWT in the query string `code`. 
* Me: If the ALB does not receive a response or unencrypted JWT, it will timeout after 2 seconds (AWS support can see the timeout in the internal log) and returns a 500
* Me: The issuer uri is matched against the `iss` field in the JWT (thus why the JWT needs to be decrypted)
* Me: The `exp` field in the token must not be more then two minutes difference (thus why the JWT needs to be decrypted)
* Me: The ALB uses https to communicate with the IDP, but does not validate the root ca (in case you are using your own internal ca)

* AWS: Customer supplied parameters
 1. Issuer (uri starting with https and does not have a trailing /)
 2. AuthorizationEndPoint (uri starting with https and does not have a trailing /)
 3. TokenEndPoint
 4. UserInfoEndpoint
 5. ClientId
 6. ClientSecret
* Optional
 1. SessionTimeout (604800)
 2. SessionCookieName (AWSELBAuthSessionCookie) User will be be authenticated if cookie does not exist.
 3. Scope (must at least be `openid`, but other scopes can be added to get more user information)
 4. AuthentiationRequestExtraParams (You can find these in the OpenID standard specification; however, `acr_values` is common)
 5. OnUnauthenticatedRequest (deny)

### Test should mirror ALB requirements and behavior
* Enter only the same endpoints as used by AWS setup
* Rely on idp to authenticate user
* For any user if authentication is successful continue to on to display a web page containing the user info or ideally the originally requested url.
