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
