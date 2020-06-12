---
title: "Aurora"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /aurora/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## What is Aurora?

- Compatible with Postgres & MySQL.
- Proprietary AWS technology.
- Cloud optimized.
- 5x performance improvement over MySQL on RDS, and 3x Postgres on RDS.
- Uses auto-scaling storage.
- 10GB, upto 64TB.
- Supports up to 15 read rpelicates (sub 10ms replica lag).
- Instantaneous failover.
- HA by default.
- 20% more than RDS.
- Only one Aurora instance (master) takes writes.

## High Availability

- Uses shared storage volume, to store 6 copies of data across 3 AZs.
- Only needs 4 copies out of 6 for writes, and 3 out of 6 for reads.
- Self healing with peer-to-peer replication to remediate bad data.
- Storage striped across 100s of volumes.
- Automated failover for master in less than 30seconds.
- Supports cross region replication.

## Read Scaling

- Read replicas support auto-scaling.

## Aurora DB Cluster

- DNS name (writer endpoint) always pointing to master to write data.
- Reader endpoint manages connection load balancing to connect client to one of the read replicas.
- Load balancing is done at the connection level.

## Features

- Automatic failover.
- Backup & recovery.
- Isoltion & security.
- Industry compliance.
- Push button scaling.
- Automated patching with no downtime.
- Advanced monitoring.
- Routine maintenance.
- Backtrack.

## Security

- Encryption at rest via KMS.
- Automated backups, snapshots and replicas are encrypted.
- Encryption in transit via SSL.
- Can authenticate using IAM tokens.
- User responsible for configuring security groups.
- Can't SSH into instance.

## Aurora Serverless

- Automated db instantiation and auto-scaling based on actual usage.
- Good for infrequent, intermittent or unpredictable workloads.
- Removes the need for capacity planning.
- Pay per second, can be more cost effective.
- Clients connect to a proxy fleet, that proxy the comms to the Aurora instances.

## Global Aurora

- Cross-region read replicas.
  - Good for DR.
  - Easy to setup.
- Aurora Global Database
  - 1 primary region for read/write.
  - Up to 5 secondary read only regions.
  - Replication lag < 1second.
  - Up to 16 read replicas per secondary region.
  - Decreases latency.
  - Promoting to another region due to a DR event takes < 1min.
