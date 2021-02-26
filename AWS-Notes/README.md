# AWS Notes
## Logging
Logging can be accomplished in many ways, but here we will compare an alternate way of getting the same information into Splunk.
### The Splunk Way
The basic Splunk configuration would be a Splunk agent on a server/instance that sends data to a Splunk Heavy Forwarder (not a splunk expert so that may be the incorrect name)
### AWS to Splunk (S3)
The (new) CloudWatch agent can now read files as well as metrics from an instance and send the data to a log group. The files have to be specifically selected, but the behavior can be made to emulate the Splunk agent with enough care. Further event bridge and ssm could probably be used to autoconfigure the paths based on instance tags.
The data from the log group can then be sent to S3 and Splunk is triggered to poll it, or the data can be sent to firehose.
### CloudWatch Alerts to Splunk HEC (Http Event Collector)
Create an alert with the target of a sqs queue, which then has events dequeued by sns. Sns then forwards to a Lambda which adds suplementry information such as the region, account number (from arn) or something implied from the sns name. Then put that into a json format and use it as the event data.
```
{
  "time": 123, # Time since epoch
  "host": "a.reporting.instance.local", # could be arn of reporting lambda
  "source": "something", # For instance the log group name
  "sourcetype": "aws:cloudwatch", # something like that
  "index": "aws", # or aws account number or whatever
  "event": {
    "field1": "value1",
    "field2": "value2"
  }
}
```
  
### Other Ways Splunk Gets Data
Syslog data to a Splunk Forwarder
JSON data to a Splunk HTTP Event Collector (HEC)
Splunk reading an S3 bucket when notified that the S3 bucket has new data.
Splunk reading an S3 bucket with Firehose
Splunk has application specific agents - such as logging into a database server and querying their logs as a database user.
Splunk integration with 3rd party products like Nessus.
