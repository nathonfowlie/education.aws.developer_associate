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
- 



