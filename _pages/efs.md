---
permalink: /efs/
toc: true
toc_label: "Table of Contents"
toc_icon: "book-reader"
sidebar:
  nav: "docs"
---

## What is Elastic File System (EFS)?

- Managed NFS volumes that can be mounted on many EC2 instances, across many AZs.
- Very expensive.
- Pay based on what is used (automatic scaling).
- Highly available.
- Good for content management, web serving, data sharind, wordpress.
- Uses NFSv4.1.
- Uses security groups to control access.
- Only compatible with linux.
- Encryption at rest with KMS keys.
- Thousands of concurrent clients, with 10GB/sec throughput.
- Can grow to PB scale file systems.
- Performance mode is set at creation time.
  - General purpose (default): latency sensitive use cases (web servers etc).
  - Max IO: higher latency, highly parallel (big data, media processing).
- Can use lifecycle management to remove files after a number of days.
- Throughput mode has two options
  - Bursting
  - Provisioned
- Performance mode has two options
  - General purpose
  - Max I/O
- Can have a public or private IP.
- Two modes
  - Standard: Frequently accessed files.
  - Infrequent Access (EFS-IA): cheaper to store files, but additional cost to retrieve files.

## Mounting EFS volumes

- Need to install the amazon-efs-utils package or nfs-utils.
- Mounting using efs helper, or nfs client.
- Make sure security group allows NFS port 2049.
