---
title: "Elastic File System"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /efs/
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
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

## Cleaning up EFS

- Make sure volumes are removed after deleting EFS volume.
- Make sure security group(s) are cleaned up.

## EBS vs EFS

- EBS can only be attached to one instance at a time. EFS can attach to multiple instances.
- EBS is bound to a specific AZ, EFS is multi-AZ.
- EFS only for linux (NFS).
- EFS is more expensive than EBS.
- Can use EFS-IA to save money via lifecycle policy.
- EFS billed for what you use. EBS you pay for provisioned capacity.
- gp2 IO increases if the disk size increases.
- io1 can increase IO independantly of disk size.
- To migrate EBS volume across AZ, take a snapshot, and restore into the new AZ. Uses alot of IO.
- EBS will be terminated by default when the instance is terminated (can be turned off).
