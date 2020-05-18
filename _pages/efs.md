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
- Storage tiers using lifecycle management.
  - Standard: Frequently accessed files.
  - Infrequent Access (EFS-IA): cheaper to store files, but additional cost to retrieve files.
