# To-Do

Ideas for things to do

#### Realtime alerting

- Done: Add a Java tail implementation + Filter (grep kind of) + Slack client (raw or maybe Retrofit 2)
- Done: Combine tail implementation with filter and Slack notifier from VPN notes project.
- Done: Push to cloudtrail -> cloudwatch event -> SNS -> Lambda (https json serialization) -> Splunk/Slack/Whatever
- Push a button to start your web app: Fliq or (Amazon equivalent) -> API Gateway -> Lambda (eg. a Graal microserivce framework) -> Whatever

#### Cross Region Session State Implementation

- Many of AWS session state recommendations are not cross region. Figure out how to use something like DynamoDB or Spring Sessions.

#### Microservices and API Gateway and S3

- Might be fun to write some Lambdas to test with API gateway.
- Might be fun to write Vertx REST serice in an ECS or EKS container fronted by API Gateway
-

##### Random notes: Java Microservice frameworks with Graal (not Kotlin or Scala right now - so they will work with Lambda)

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
3. What is the LOE to integrate Vertx with Spring Session
4. Scheduled cloudwatch event to purge old sessions (from a session store db)

##### Really really old notes that do not even apply to Vert.X 5

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
```

```
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

- The focus of this will be microservices which can function with Graal. (Go/Rust/Python directly supported in Lambdas, so while some Rust microserivce frameworks look great, I will not be exploring those now)
- Starting with TechEmpower benchmarks (update - what happened to the benchmarks? Their page has not been updated in a while. The use case is for sites with such high demand that they cannot use containers, and instead are trying to use the least instances to handle the most load. In some cases the highest performing server may not be relevant, but rather its ability to integrate into a stack. At any rate it was still nice to see the top contenders.)

##### Demo using client side single page app to microservices behind API Gateway (Still high on my priority list)

1. Something in Angular2, React, Vue, or similar

#### New Languages To Learn

- AST reasearch

#### Random Cloud Container Notes

- Process supervisors which do not use cgroups and are meant to run as pid 1: S6, Runit
  1. AWS ECS Fargate injects useful metadata into `/proc/1/environ` so it might be important to retain pid 1 environ. which can be parsed with Strings
- (S6 Overlay)[https://github.com/just-containers/s6-overlay] (looks like it could be really great but the learning curve is high)
- See BusyBox page on removing systemd
- Minimalistic init's to prevent zombies and resource leaks: tini/dumb-init/minit (Originally at `https://github.com/chazomaticus/minit`)/(or any other simple init)
- Example project of a Docker container with a Process Supervisor, which start multiple processes such as (S6 Overlay)[https://github.com/just-containers/s6-overlay]
- socklog (deal with missing /dev/log)
- Note AL2 is EOL: https://github.com/aws/aws-codebuild-docker-images/tree/master/al2/x86_64/standard/3.0 (this also does dind)
- Systemd does not work in containers without extra capabilities for a number of reasons such as cgroups, and dbus - but it seems to be the standard most applications use. Good thing there is a systemctl replacement just for containers
- Although I may not like it some people have oddly made software packages require systemd
  1. systemd (systemctl) (docker-systemctl-replacement)
  2. rsyslog
  3. logrotate
  4. binutils
  5. coreutils
  6. python3
  7. iptables
  8. util-linux
  9. iproute
  10. net-tools
  11. maybe ethtool
  12. dbus
  13. maybe java

#### Add ECS/ECR/Fargate notes

- Add notes
- Find out how to do sidecars
  1. https://linkedin.com/pulse/architecting-sidecar-securely-aws-fargate-anuj-gupta?trk=public_profile_article_view
  2. https://blog.aquasec.com/securing-aws-fargate-with-sidecards
  3. https://aws.amazon.com/blogs/compute/nginx-reverse-sidecar-container-on-amazon-ecs/
  4. https://blog.aquasec.com/revisiting-aws-fargate-with-aqua-3.0
- Add another entry to the array of images in the Task Definition. These can then share resources, but run as two seperate containers.
  1. Good for being able to upgrade one container image without affecting the other.
- An ECS Fargate Metadata to EC2 Metadata mock. Some software assumes EC2, doesn't understand where to get ECS metadata. A mock that combines iptables, the ECS Metadata and some API calls to produce something that looks like EC2 Metadata would be useful for software compatibility.

#### Jenkins

- Add some Jenkins DSL and Groovy notes.

#### Projects I'm interested in because of the ESRI ArcGIS work I have done:

- Various projects related to election data both sources and visualization
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

#### Random

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

- Validate checksum in iterator and add new 800-87r2 codes
- https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-87r2.pdf
- https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.201-2.pdf
- https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-73-4.pdf
- https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-78-4.pdf
- https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-76-2.pdf
- https://www.idmanagement.gov/wp-content/uploads/sites/1171/uploads/TIG_SCEPACS_v2.3.pdf

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

#### React Notes

```
Make sure your libraries are compatible with the version of react you have installed (lets say 18+)
React bootstrap modules: read-scripts, react
Some libaries which are interesting: next, axios, react-router, react-dom, react-arborist
```

#### Message Queue Software

- RocketMQ
- Kafka
- Sequential consumer reads
- How would message dedupe work
