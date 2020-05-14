---
permalink: /elb/
toc: true
toc_label: "Table of Contents"
toc_icon: "book-reader"
sidebar:
  nav: "docs"
---

## Scalability

- Handle greater workloads by adapting.
- Two types - vertical, and horizontal (elasticity).

### Vertical Scalability

- Increase the instance size (moar hardware!).
- Common for non-distributed systems (databases, elasticache etc).
- Limited scalability. Bound to the available hardware.

### Horizontal Scalability (Elasticity)

- Increase instance count (use ALL the infra!).
- Common for web apps, distributed applications.
- Use auto-scaling groups.

## High Availability

- Run application in multiple locations to support single site loss (multi AZ).
- Can be active or passive
- Auto scaling group (multi AZ)
- Load balancer (multi AZ)

### Passive

- Multiple locations with stand-by infrastructure.

### Active

- Multiple locations with all infrastructure in active use.
