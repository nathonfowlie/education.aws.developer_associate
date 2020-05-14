---
permalink: /ec2/
toc: true
toc_label: "Table of Contents"
toc_icon: "book-reader"
sidebar:
  nav: "docs"
---

## What is EC2?

- Launch/manage VMs.
- Store data on EBS.
- ELB for load balancing.
- Manage auto-scaling groups (ASG).

## Availability Zones

- Physical data centres.
- Isolated power suppliers, cooling, power etc.
- Usually at least 3 per region.

## Regions

- Geographical location with multiple availability zones.

## Pricing

- Hourly prices are based on:
    1. Region
    2. Instance type
    3. On-Demand vs Spot vs Reserved vs Dedicated hosts.
    4. IS
    5. Lots more...
- Billed by the second, with a minimum of 60seconds.
- Don't pay for stopped instances.
- Additional cost for storage, data transfer, public/private IPs, load balancing etc.

## Amazon Machine Image (AMI)

- Base image.
- EC2 User Data can be used to customise the image.
- AMIs are bound to a specific region.

### Why use a Custom AMI?

- Pre-install required packages.
- Faster boot time by avoiding EC2 User Data.
- Pre-configure monitoring/enterprise software.
- Pre-install applications for faster deployments.
- Support Active Directory out of the box.
- Security concerns (need more control of the image).
- Control the maintenance & updates of AMIs over time.
- Optimised for a specific purpose.
- 

## EC2 User Data

- Runs once, the first time an instance is started.
- Runs as root.
- Script is base64 encoded.
