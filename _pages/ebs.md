---
title: "EBS"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /ebs/
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
sidebar:
  nav: "docs"
---

## What's an EBS Volume?

- An EC2 machine loses its root volume when it's manually terminated.
- EBS is a network attached volume to persist data.
- Can be de-ttached and re-attached to another EC2 instance in the same AZ.
- To move volumes between AZs, need to take a snapshot first.
- Have provisioned capacity (IOPS, and volume size).
- Billed for all provisioned capacity, even if it's not used.
- Capacity can be increased as needed.
- Only GP2 and IO1 can be used as boot volumes.

## EBS Attributes

- Size
- Throughput
- IOPS

## EBS Volume Types

| Volume Type | Optimised for                 | Boot Volume | Uses                                                                                                                                  | Size       | IOPS                                                                       |
|-------------|-------------------------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------|------------|----------------------------------------------------------------------------|
| GP2 (SSD)   | Balance price vs performance. | Yes         | General purpose.<br/>Low latency interactive apps<br/>Virtual desktops<br/>Dev/Test environments<br/>Recommended for most workloads.  | 1GB-16TB   | 3 IOPS/TB<br/>Minimum 100 IOPS with burst to 3000. Maximum 16,000 IOPS.    |
| IO1 (SSD)   | Provisioned IOPS              | Yes         | Large workloads & critical business applications - DB servers etc.<br/>Highest performance SSD.<br/>Expensive.                        | 4GB-16TB   | 50IOPS/GB<br/>Minimum 100 IOPS, upto 32,000. Or 64,00 with Nitro instance. |
| ST1 (HDD)   | Streaming workloads           | No          | Kafka, data warehouses, log processing. Low cost.                                                                                     | 500GB-16GB | Maximum 500 IOPS<br/>Maximum 500MB/sec through-put.                        |
| SC1 (HDD)   | Infrequently accessed data    | No          | Cheapest. Good for cold backups, snapshots.                                                                                           | 500GB-16GB | Maximum 250GB/sec with burst.                                              |

## Instance Store

- Some instances don't come with root EBS volumes.
- Instance stores are ephemeral storage.
- Physically attached to the machine.
- Provides better IO performance (up to millions on IOPS).
- Good for buffering/caching/scratch data/temporary content.. Data survives reboots.
- Instance store is lost on termination and can't be resized. Backups need to be operated by the user.
- Up to 7.5TB, striped them to reach 30TB.
- Viewed on the instance as block storage.
- Risk of data loss if hardware fails.
