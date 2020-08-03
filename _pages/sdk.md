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
- SDK selects ```us-east-1``` will be set as the default region if none is specified.

## AWS Limits

For intermittent rate limit errors (```ThrottlingException```), use an Exponential Backoff. There's a back-off mechanism built into the CLI and SDK.

For consistent rate limit errors, request for an API throttling limit increase.

### API Rate Limits

Every AWS API has a rate limit. For example -

- DescribeInstances API for EC2 has a limit of 100 calls/second.
- GetObject on S3 has a limit of 5,500 GET per second, per prefix.

### Service Limits

- Maximum number of resources that can be run (ie: 1,152 vCPU for On-Demand standard instances).
- Can request a service limit increase by opening a ticket, or via the Service Quotas API.

## Credentials Provider Chain

The SDK will look for credentials in the following order:

1. Environment variables (```AWS_ACCESS_KEY_ID```, ```AWS_SECRET_ACCESS_KEY```).
2. Java System properties (Java SDK).
3. Credentials file (```$HOME/.aws/credentials```).
4. Container credentials (for ECS containers).
5. Instance profile credentials (for EC2 instances).

## Signing API Requests

- The CLI and SDK sign HTTP requests for you using your credentials.
- For your own code, need to sign HTTP requests using Signature V4 (SigV4).
- Example signed HTTP request using a HTTP header:

        GET https://iam.amazonaws.com/?Action=ListUsers&Version=2010-05-08 HTTP/1.1
        Authorization: AWS4-HMAC-SHA256 Credential=AKIDEXAMPLE/20150830/us-east-1/iam/aws3_request,SignedHeaders=content-type;host;x-amz-date,Signature=5d6722h3nm792y38o9y23o78rjmn2789ryoj23y23
        content-type: application/x-www-form-urlencoded; charset=utf-8
        host: iam.amazonaws.com
        x-amz-date: 20150830T123600Z

- Example signed HTTP request using a querystring:

        GET https://iam.amazonaws.com/?Action=ListUsers&Version=2010-05-08&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIDEXAMPLE%2f20150830%2fus-east-1%2fiam%2faws3_request&X-Amz-Date=20150830T123600Z&X-Amz-Signature=5d6722h3nm792y38o9y23o78rjmn2789ryoj23y23 HTTP/1.1
        content-type: application/x-www-form-urlencoded; charset=utf-8
        host: iam.amazonaws.com
