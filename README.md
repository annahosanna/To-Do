# To-Do
Ideas for things to do

#### Loadbalancer support?
* Adapt URL fetch to be a a general purpose URL health check (I'm not sure why anymore).

#### Realtime alerting
* Add a Java tail implementation + Filter (grep kind of) + Slack client (raw or maybe Retrofit 2)
* Combine tail implementation with filter and Slack notifier from VPN notes project.
* Push to cloudtrail -> cloudwatch event -> SNS -> Lambda (https json serialization) -> Splunk/Slack/Whatever
* Fliq or (Amazon equivalent) -> API Gateway -> Lambda (eg. a Graal microserivce framework) -> Whatever
  1. Microservice is cross region
  2. Keeps track of shared session state in DynamoDB (Perhaps using Spring Session?)  
* Some SLF4J notes

#### Microservices and API Gateway and S3
1. Prefer if I could test in a local container
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

##### Things to explore
1. Spring Security
2. Spring Session
3. Does Vertx need to be wrapped with an HttpSession interface for Spring Session
4. Scheduled cloudwatch event to purge old sessions
5. es4x + Dart Redstone

##### API Gateway + Lambda Notes
* The focus of this will be microservices which can function with Graal. (Go/Rust/Python directly supported in Lambdas, so while some Rust microserivce frameworks look great, I will not be exploring those now)
* Starting with TechEmpower benchmarks

##### Demo using client side single page app to microservices behind API Gateway
1. Something in Angular2 or similar
##### Notes for hosting a static website on S3
1. Route 53
2. Software to create static websites
#### Container stuff
* Notes with regard to Oracle Linux and using S6, Runit, and minit.
* Turn minit ```https://github.com/chazomaticus/minit``` into a process supervisor. (I'm not sure why a per process supervisior is useful)

#### Jenkins
* Add some Jenkins DSL and Groovy notes.
* Add JDBC Ping tool (Fail fast for db integration testing/liquibase)
  1.  Inspired by [https://github.com/OHDSI/WhiteRabbit/blob/master/src/org/ohdsi/databases/DBConnector.java]

#### Inspired by ESRI ArcGIS work I have done:
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

#### Qbit is old. Use a lighter microservice framework
* Qbit was kind of interesting in how many things it glued together (health, swagger, graphite stats, annotations to describe pages w/o beans)
* The documentation was incomplete or out of date, but that was ok because it was open source.
* Qbit
  1.  Fix the Consul functionality in Qbit
  2.  Add some notes on using Promise.asHandler
  3.  Move Qbit Consul functionality from blocking HTTP calls to non blocking.
  4.  Add Qbit Vertx example

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
