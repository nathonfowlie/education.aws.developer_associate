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
