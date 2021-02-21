---
title: "SNS"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /sns/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## Overview

- Pub/Sub service.
- Allows multiple subscribers to receive a message from the SNS topic (queue).
- Event Producer only sends a message to one SNS topic.
- One or more Event Recievers (subscriptions) listen to the SNS topic.
- Each subscription to the topic will get all messages. Messages can optionally be filtered.
- Up to 10million subscriptions per topic.
- Up to 100,000 topics.
- Subscribers can be SQS, HTTP/HTTPS, Lambda, Email, SMS messages or Mobile notifications.
- Most AWS services can send data to SNS for notifications.

## Publishing

### Topic Publish (Using the SDK)

- Create a topic.
- Create a subscription(s).
- Publish to the topic.

### Direct Publish (for mobile apps SDK)

- Create a platform application.
- Create a platform endpoint.
- Publish to the platform endpoint.
- Works with Google GCM, Apple APNS, Amazon ADM etc.


## Security

### Encryption

- Encryption during transit via HTTPS.
- Encryption at rest using KMS.
- Client-side encryption if desired.


### Access Controls

- IAM policies regulat access to the SNS API.
- SNS Access Policies, similar to S3 Bucket Policies for cross-account access to SNS topips, or allowing other service to write to an SNS topic.

