---
title: "X-Ray"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /xray/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## Overview

- Visual analysis of applications, particularly useful for micro service architecture.
- Helps to troubleshoot application performance, dependencies, pinpoint service issues, review request behaviour, find errors/exceptions, check SLAs and where the throttling is happening.
- Helps identify the users impacted by errors.
- Compatible with Lambda, Elastic Beanstalk, ECS, ELB, API Gateway, EC2 instances, application servers (including on-prem).
- Leverages tracing to follow a request (business transaction). Each component adds it's own "trace". Tracing is composed of segments.
- Annotations can be added to traces to provide extra information.
- Can control trace interval (every request, rate per minute, % of requests etc).
- IAM for authorization, KMS for encryption at rest.

### Segments

- Send by the application/service.
- Use sub-segments if more details are required in the segment.
- Calls made between different application components.

### Trace

- Collection of segment documents that represents an end to end trace (business transaction).
- X-Ray daemon/agent needs extra configuration to send traces cross account:
  - IAM permissions need to be correct, because the agent will assume a role.
  - Allows a single account to be used for application tracing.

### Sampling

- How frequently requests are sent to X-ray.
- Reduce the sampling rate to reduce cost.

### Sampling Rules

- Doesn't require code changes.
- By default, the first request each second (reservoir), and 5% of subsequent requests (rate) are recorded.
- The reservoir and rate can be modified using custom sampling rules, to control the amount of data being recorded.
- Lower priority value has precedence (ie: priority 1 has precedence over another custom sampling rule with priority 5000).

### Annotations

- Key-value pairs that can be used to index traces, and use with filters.

### Metadata

- Key-value pairs that are not indexed and can't be used for searching.

## Enabling AWS X-Ray

- Application code needs to import the AWS X-Ray SDK. Must be Java, Python, Go, Node.JS or .NET.
- AWS Application SDK will capture:
  - Calls to AWS services.
  - HTTP/HTTPS requests.
  - Database calls (MySQL, PostgreSQL, DynamoDB).
  - Queue calls (SQS).
- Need to install the X-Ray daemon (low level UDP packet interceptor), or enable X-Ray AWS integration (Lambda etc).
- Application needs IAM rights to write data to X-Ray.
- To enable on Elastic Beanstalk, use ```.ebextensions/xray-daemon.config```.

## X-Ray Troubleshooting

- Check the EC2 IAM role has proper permissions.
- Check the EC2 instance is running the X-Ray daemon.
- Ensure Lambda has an IAM execution role with the proper policy (```AWSX-RayWriteOnlyAccess```).
- Ensure X-ray is imported into the application code.

## X-Ray Instrumentation

```
var app = express();

var AWSXRay = require('aws-xray-sdk');
app.use(AWSXRay.express.openSegment('MyApp'));

app.get('/', function (req, res) {
  res.render('index');
  });
  
  app.use(AWSXRay.express.closeSegment());
```

- Use the X-Ray SDK.
- Can add annotations to the data sent to X-ray,  use interceptors, filters, handlers, middleware etc.

## X-Ray APIs

### X-Ray Write API

- Used by the X-Ray daemon to send data to AWS X-Ray.
- Needs the right IAM policy that permits writing to X-Ray.
- ```PutTraceSegments```: Upload segments to the X-Ray service.
- ```PutTelemetryRecords```: Used by the X-Ray daemon to send telemetry - segments received/rejected count, connection errors etc.
- ```GetSamplingRules```: Retrieve all sampling rules (to know what/when to send).
- ```GetSamplingTargets```: Also related to sampling rules.

### X-Ray Read API

- ```GetSamplingRules```: Retrieve all sample rules (to know what/when to send).
- ```GetSamplingTargets```: Also related to sampling rules.
- ```GetSamplingStatisticSummaries```:  Also related to sampling rules.
- ```BatchGetTraces```:  Retrieve a list of traces specified by ID. Contains full trace information.
- ```GetServiceGraph```: Get the main graph visible in the console.
- ```GetTraceGraph```: Get the service graph for one or more specific trace IDs.
- ```GetTraceSummaries```: Retrieve the ID and annotations for traces available within a specified time. Can be filtered.
- ```GetGroups```: ???
- ```GetGroup```: ???
- ```GetTimeSeriesServiceStatistics```: ???

## X-Ray with Elastic Beanstalk

- Enable the X-Ray daemon via the Elastic Beanstalk console, or
- Enable the X-Ray daemon via ```.ebextensions/xray-daemon.config```:
    ```
    option_settings:
      aws:elasticbeanstalk:xray:XRayEnabled: true
    ```
- Assign the right IAM policy to the instance id to allow Elastic Beanstalk to send data to AWS X-Ray.
- Instrument your code to collect segments, create traces etc.

## X-Ray with ECS

```
{
  "name": "xray-daemon",
  "image": "123456789012.dkr.ecr.us-east-2.amazonaws.com/xray-daemon",
  "cpu": 32,
  "memoryReservation": 256,
  "portMappings": [
    {
      "hostPort": 0,
      "containerPort": 2000,
      "protocol": "udp"
    }
  ],
},
{
  "name": "scorekeep-api",
  "image": "123456789012.dkr.ecr.us-east-2.amazonaws.com/scorekeep-api",
  "cpu": 192,
  "memoryReservation": 512,
  "environment": [
    { "name": "AWS_REGION", "value": "us-east-2" },
    { "name": "NOTIFICATION_TOPIC", "value": "arn:aws:sns:us-east-2:123456789012:scorekeep-notifications" },
    { "name": "AWS_XRAY_DAEMON_ADDRESS", "value": "xray-daemon:2000" }
  ],
  "portMappings": [
    {
      "hostPort": 5000,
      "containerPort": 5000
    }
  ],
  "links": [
    "xray-daemon"
  ]
}
```

### ECS Cluster

- Run the X-Ray Container as a Daemon on each EC2 instance, OR
- Run the X-Ray Container using the side-car pattern.

### Fargate Cluster

- Run the X-Ray Container using the side-car pattern.
