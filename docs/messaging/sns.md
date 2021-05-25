# Simple Notification Service (SNS)

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

### Subscription Protocols

- HTTP
- HTTPS
- Email
- Email-JSON
- Amazon SQS
- Amazone Lambda

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

## SNS + SQS Fan Out Pattern

- Push a message to SNS, and it will be recieved by all SQS queues that are subscribers.
- Fully de-coupled, no data loss.
- SQS allows data persistence, delayed processing and retries of work.
- Easy to add more SQS subscribers over time. Useful for micro-services.
- SQS Queue accesspolicy needs to allow SNS to write.
- SNS can't send messages to SQS FIFO queues.

## S3 Events to Multiple Queues

- Can only have on S3 Event rule for the same event type (eg: object create) & prefix (eg: images).
- Use Fan-out pattern to send the same S3 event to many SQS queues.
