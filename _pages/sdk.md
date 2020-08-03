---
title: "AWS SDK"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /sdk/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## Overview

- Run AWS actions without using the CLI.
- Available in multiple languages:
  - Java
  - .NET
  - Node.js
  - PHP
  - Python (boto3/botocore)
  - Go
  - Ruby
  - C++
- SDK selects us-east-1 will be set as the default region if none is specified.

## AWS Limits (Quotas)

### API Rate Limits

Every AWS API has a rate limit. For example,

- DescribeInstances API for EC2 has a limit of 100 calls/second.
- GetObject on S3 has a limit of 5,500 GET per second, per prefix.

For intermittent rate limit errors (ThrottlingException), use an Exponential Backoff. For consistent errors, request for an API throttling limit increase.
  
### Service Quotas (Service Limits)

- Maximum number of resources that can be run (ie: 1,152 vCPU for On-Demand standard instances).
- Can request a service limit increase by opening a ticket, or
- Can request a service quotes increase via the Service Quotas API.

### Exponential Backoff

- Retry mechanism built into the SDK.


## SDK Default Credentials Provider Chain

The SDK will look for credentials in the following order:

1. Environment variables (```AWS_ACCESS_KEY_ID```, ```AWS_SECRET_ACCESS_KEY```).
2. Java System properties (Java SDK).
3. Credentials file (```$HOME/.aws/credentials```).
4. Container credentials (for ECS containers).
5. Instance profile credentials (for EC2 instances).
