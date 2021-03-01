---
title: "Kinesis"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /kinesis/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## Overview

- Managed alternative to Kafka.
- Data is replicated across 3 AZ.
- Can quickly become expensive.

### Use Cases

- application logs, metrics IoT, Clickstreams.
- "Real-time" big data.
- Streaming processing frameworks (Spark, NiFi).
- Shard level metrics can be in 1minute intervals.

## Kinesis Streams

- Low latency streaming ingestion at scale.
- Streams are divided into ordered Shards/Partitions. Increase shards to increase throughput.
- Data retention is 1 day by default. Can be up to 7 days.
- Can reprocess/replay data.
- Multiple applications can consume the same stream.
- Real-time processing with scale of throughput.
- Data inserted into Kinesis can't be deleted (immutable).

### Shards

- Write 1MB/s, or 1000 messages per shard.
- Read 2MB/s per shard.
- Can provision up to 200 shards.
- Billing is per shard (provisioned).
- Messages can be batches.
- Can reshard/merge shards over time.
- Records are ordered per shard.

### Kinesis Analytics

- Real-time analytics on streams using SQL

### Kinesis Firehose

- Load streams into S3, Redshift, ElasticSearch etc.

## Producers

- Put records are used to send data to Kinesis.
- Need to provide the partition key.
- The message key is hashed to determine the shard id.
- The same key goes to the same partition (helps with ordering for a specific key).
- Message data must be base64 encoded.
- Each message is given a sequence number (sequential).
- Use a partition key that's highly distributed to avoid a "hot partition", which would result in all the data being sent to the same shard.
  - user_id if many users.
  - country_id not a good choice is all the users are in one country.
- Use Batching to reduce cost and increase throughput.

### Handling ProvisionedThroughputExceeded Exceptions

- Happens when MB/s or TPS are exceeded on a shard.
- Three solutions to the problem:
  1. Retries with backoff.
  2. Increase the shards (scaling).
  3. Use a more distributed (unique) partition key.

## Consumers

- Can use the SDK, CLI or Kinesis Client Library.
- Kinesis Client Library uses DynamoDB to checkpoint offsets, track other workers, and share the work amongst available shards.
- Pass the shart iterator to ```get records --shard-iterator``` to retrieve the messages for a given shard id.

## Kinesis Client Library (KCL)

- Java library for distributed applications that helps read records from Kinesis Streams, to share the read workload across multiple nodes.
- Each shard can only be ready by one KCL instance. ie: if there's 4 shards, the maximum number of KCL instances is 4.
- DynamoDB is used to checkpoint progress and co-ordinate consumption of shards across the KCL instances (requires IAM access to write to DynamoDB).
- KCL can run on EC2, Elastic Beanstalk, On-Prem applications etc.
- Record are read in the order that they were placed into the shard.

## Security

- IAM policies to control access/authorization.
- VPC endpoints are available to allow access to Kinesis from within a VPC.

### Encryption 

- Encryption in transit via HTTPS.
- Encryption at rest using KMS
- Possible to use client side encryption.

## Kinesis Data Analytics

- Real-time analytics on Kinsesis Streams using SQL.
- Auto-scales.
- Fully managed, no services to provision.
- Continuous analytics (real time).
- Pay for actual consumption.
- Create streams out of real-time queries.

## Kinesis Firehose

- Fully managed, no administration.
- Near real time (60 seconds delay).
- Used for ETLs. Load data into Redshift/S3/ElasticSearch/Splunk.
- Automatic scaling.
- Supports alot of data formats. There's a conversion fee to convert between formats.
- Pay for the amount of data going through Firehose.

## SQS vs SNS vs Kinesis

### SQS

- Consumers pull data.
- Data is deleted once consumed.
- Can have as many consumers as you want.
- Throughput isn't provisioned.
- Ordering is not guaranteed unless you use a FIFO queue.
- Messages can be delayed if required.

### SNS

- Data is pushed to subscribers (pub/sub model).
- Up to 10m subscribers.
- Data is not persisted, it's lost if it isn't delivered.
- Up to 10,000 topics.
- Throughput is not provisioned.
- Integrates with SQS for fan-out architecture.

### Kinesis

- Consumers pull data.
- Can have as many consumers are you want.
- Data can be replayed.
- Meant for real-time big data, analytics and ETL.
- Records within the shard are ordered according to partition key.
- Data expires after X days.
- Throughput is provisioned.

## Kinesis Data Ordering vs SQS FIFO Queue

- Use the partition key to  control the order of messages in a Kinesis Stream.
- The same partition key will always go to the same shard in a Kinesis Stream.
- SQS standard isn't ordered at all.
- SQS FIFO messages without a group id will be consumed by a single consumer in the order they're sent.
- SQS FIFO messages with a group id, will be consumed by multiple consumers (one consumer per group, messages processed in the order they arrive for that group. Very similar to a Kinesis partition key).

### Example

There are 100 trucks sending GPS data, 5 kinesis shards and 1 SQS FIFO.

#### Kinesis Data Stream

- On average, 20 trucks per shard (x 5 shards).
- Data is ordered within each shard.
- Maximum of 5 consumers (one per shard).
- Up to 5MB/s of data (1MB/s per shard).

#### SQS FIFO

- 100 Group IDs (one per truck).
- Data is ordered within each group id (or in the order they arrive into the queue if no group id).
- Maximum of 100 consumers (one per Group ID).
- Up to 300 messages/second, or 3,000 if using batching.
