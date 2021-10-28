# To-Do
Ideas for things to do

#### Realtime alerting
* Add a Java tail implementation + Filter (grep kind of) + Slack client (raw or maybe Retrofit 2)
* Combine tail implementation with filter and Slack notifier from VPN notes project.
* Push to cloudtrail -> cloudwatch event -> SNS -> Lambda (https json serialization) -> Splunk/Slack/Whatever
* Fliq or (Amazon equivalent) -> API Gateway -> Lambda (eg. a Graal microserivce framework) -> Whatever
  1. Microservice is cross region
  2. Keeps track of shared session state in DynamoDB (Perhaps using Spring Session?)  
* Some SLF4J notes

#### Cross Region Session State Implementation
  * Many of AWS session state recommendations are not cross region. Figure out how  to use something like DynamoDB.
 
#### Microservices and API Gateway and S3
  * Might be fun to write some Lambdas to test with API gateway. 
  * Might be fun to write Vertx REST serice in an ECS or EKS container fronted by API Gateway
  * 
##### Java Microservice frameworks with Graal (not Kotlin or Scala right now)
1. Vert.X
2. Javalin
3. SparkJava
4. Micronaut
5. Ratpack
6. Pippa
7. Quarkus?
8. Helidon?
9. Langon?
10. Netifi?
11. Greenlightning
12. Restexpress
13. Armeria
14. Wizzardo
15. Vaaladin
16. Firefly

##### Things to explore
1. Spring Security
2. Spring Session
3. Does Vertx need to be wrapped with an HttpSession interface for Spring Session
4. Scheduled cloudwatch event to purge old sessions (from a session store db)
5. es4x + Dart Redstone?

##### Put these quick notes into something
```
final CountDownLatch latch = new CountDownLatch(1);
final Vertx vertx = Vertx.vertx();
vertx.deployVertical(new MyVertxInitClass(data), result -> {
if (result.succeeded()) {
// yay
} else {
result.cause().printStackTrace();
}
latch.countDown();
});

latch.await(5, TimeUnit.SECONDS);
// Yay  started

/////////////////////////////////////
class MyVertxInitClass extends AbstractVertical {
public MyVertxInitClass(MyClass data) {
this.data = data;
}

@override
public void start() thows Exception {
final io.vertx.core.http.HttpServerOptions vertxOptions = new io.vertx.core.http.HttpServerOptions();
// if ssl enabled
// JksOptions trustStore ...
// JksOptions keyStore ...
// vertxOptions.setTrustStoreOptions ...
// vartxOptions.setKeystoreOptions ...

final io.vertx.core.http.HttpServer vertxHttpServer = this.getVertx().createHttpServer(vertxOptions);
final Router router = Router.router(getVertx());
router.route("/*").failureHandler(ctx -> {
ctx.response().putHeader("Content-Type", "...").setStatusCode(...).end("body");
})
.handler(ctx -> {if (!(ctx.normalisedPath.matches(..))){
ctx.response()...
} else {
ctx.next();
}});
vertxHttpServer.requestHandler(route::accept).listen(
)
}
@override
public void stop() throws Exception {
}
}
```
##### API Gateway + Lambda Notes
* The focus of this will be microservices which can function with Graal. (Go/Rust/Python directly supported in Lambdas, so while some Rust microserivce frameworks look great, I will not be exploring those now)
* Starting with TechEmpower benchmarks

##### Demo using client side single page app to microservices behind API Gateway
1. Something in Angular2 or similar

#### New Languages To Learn
  * AST reasearch
  * TypeScript looks useful
  
##### Notes for hosting a static website on S3
1. Route 53
2. Software to create static websites

#### Container stuff
* Process supervisors which do not use cgroups and are meant to run as pid 1: S6, Runit
* (S6 Overlay)[https://github.com/just-containers/s6-overlay]
* What about tini -> bash -> Start programs -> 'wait'
* Minimalistic init's to prevent zombies and resource leaks: tini/dumb-init/minit (Originally at `https://github.com/chazomaticus/minit`)/(or any other simple init)
* Example project of a Docker container with a Process Supervisor, which start multiple processes such as (S6 Overlay)[https://github.com/just-containers/s6-overlay]
* socklog (deal with missing /dev/log)
* https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/x86_64/standard/3.0 (this also does dind)
* Systemd does not work in containers without extra capabilities for a number of reasons such as cgroups, and dbus - but it seems to be the standard most applications use.
* Need to add a link here to the explanation of why dind is so hard, and which solutions are truely rootless.
* Add ECS/ECR notes

#### Jenkins
* Add some Jenkins DSL and Groovy notes.
* Add JDBC Ping tool (Fail fast for db integration testing/liquibase)
  1.  Inspired by [https://github.com/OHDSI/WhiteRabbit/blob/master/src/org/ohdsi/databases/DBConnector.java]

#### Projects I'm interested in because of the ESRI ArcGIS work I have done:
* Various projects related to election data both sources and visualization
  1.  https://github.com/timcraft/electionmap
  2.  https://github.com/markmarkoh/datamaps
  3.  https://github.com/systers/volunteers-iOS
  4.  https://github.com/aspittel/election-map
  5.  https://github.com/geofmureithi/254-Election-Predictor
  6.  https://github.com/angrobertsh/elections
  7.  https://github.com/guardian/interactive-election-polls-2015
  8.  https://github.com/fitzk/CAVotesByCounty
  9.  https://github.com/torvarun/d3-us-election
  10. https://github.com/outragedpinkracoon/election-map-d3
  11. https://github.com/mzagaja/election_maps
  12. https://github.com/joyawood/Election-twitter-visualization
  13. https://github.com/buinyi/Udacity-Data-Visualization-with-D3.js---Election-results
  14. https://github.com/mapmeld/election-tangram
  15. https://github.com/JEverhart383/D3js-Elections-
  
#### CloudFormation examples (perhaps CDL and Teraform too)
  1.  JSON <-> YAML conversion
  2.  Template automation with FreeMarker
  3.  Encrypted cross account S3 bucket logging
  
#### CustomClosableHttpClient uses HTTP Components
* ~~Finish off~~ Stop working on CustomClosableHttpClient in VPN notes. It uses Apache HTTP Components which seem to be outdated.  Possibly switch to Netty. 

* Random
```
javalin
sparkjava
Utility library: https://google.github.io/dagger/ (Dagger 2) Enable reflections to function with graals ahead of time compilation
https://royvanrijn.com/blog/2018/09/part-2-native-microservice-in-graalvm/ (a whole bunch of work arounds)
micronaut
quarkus
helidon.io
Interesting microservice design presentation: https://static.rainfocus.com/oracle/oow19/sess/1554495896703001jsZK/PF/DEV5354%20-%20Polyglot%20gRPC%20Microservices%20With%20Helidon%20and%20Graal_1568453488620001uUpI.pdf
vert.x
es4x (nodejs) + Redstone (Dart)
narayana (transation control for web services)
jooby2?
https://spotify.github.io/apollo/
act
proteus
pippo
https://ratpack.io/
https://github.com/networknt/light-rest-4j
https://britesnow.com/snow
retrofit 2
restexpress
ratpack
gemini
minijax
jawn
play2
dropwizard
wicket
webflux
tapestry
ninja
activeweb
afterburner
cjs
curacao
pac4j
https://www.findbestopensource.com/tagged/microservice?fq=Java
vxms
dart + es4x ?
```

#### Update FASC-N in BAE-Client
Validate checksum in iterator and add new 800-87r2 codes
https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-87r2.pdf
https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.201-2.pdf
https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-73-4.pdf
https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-78-4.pdf
https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-76-2.pdf
https://www.idmanagement.gov/wp-content/uploads/sites/1171/uploads/TIG_SCEPACS_v2.3.pdf

### Stuff that I probably do not need
```
https://github.com/systers/volunteers-iOS
https://github.com/aspittel/election-map
https://github.com/geofmureithi/254-Election-Predictor
https://github.com/petewarden/iPhoneTracker
https://github.com/angrobertsh/elections
https://github.com/guardian/interactive-election-polls-2015
https://github.com/fitzk/CAVotesByCounty
https://github.com/torvarun/d3-us-election
https://github.com/outragedpinkracoon/election-map-d3
https://github.com/annahosanna/Transfiguration
https://github.com/annahosanna/election_maps
https://github.com/joyawood/Election-twitter-visualization
https://github.com/gabriel-samfira/CoreClrPSUtils
https://github.com/buinyi/Udacity-Data-Visualization-with-D3.js---Election-results
https://github.com/mapmeld/election-tangram
https://github.com/JEverhart383/D3js-Elections-
```
