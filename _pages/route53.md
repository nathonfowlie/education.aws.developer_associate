---
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /route53/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---


## Overview

- Managed DNS service.
- Most common records -
  - A (hostname to IPv4)
  - AAAA (hostname to IPv6)
  - CNAME (hostname to hostname)
  - Alias (hostname to AWS resource)
- Can use public domain names.
- Can use private domain names that can only be resolved by instances within the VPC.
- Costs $0.50/month per hosted zone.
- Global service.
- Provides advanced features like load balancing, health checks, routing policies.

## Time to Live (TTL)

- Web browser caches DNS record for given number of seconds to minimise DNS lookups.
- Default TTL is 300secs.

## Common DNS Records

### CNAME

- Points a hostname to any another hostname.
- Can't be used for root domains.

### Alias

- Points a hostname to an AWS resource (foo.amazonaws.com)
- Can be used for root domains (zone apex).
- Free of charge.
- Supports native health checks (use the health check configured on the load balancer).
- Can add multiple addresses to the record.

## Routing Policies

### Simple Routing Policy

- When you need to redirect to a single resource.
- Doesn't support health checks.
- Can return multiple values to the client, clients choses a random value to use.


### Weighted Routing Policy

- Controls the percentage of requests that go to particular endpoints.
- Useful way to test new versions of the application on a limited sub-set of requests.
- Can be associated with health checks.
