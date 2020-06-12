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

### Latency Routing Policy

- Direct to the server that has the least latency (based on AWS region).
- A user in Germany might be routed to resources in the US if that has the lowest latency.

#### Health Checks

- By default, health state changes after 3 successful/unsuccessful health checks in a row.
- 30second health check interval by default. Fast health checks happen every 10seconds but costs money.
- Around 15 health checkers will check the endpoint health. So roughly 1 request/sec.
- Supports HTTP, TCP, HTTPS (no SSL verification) healthchecks.
- Can integrate healthcheck with CloudWatch.
- Can use string matching, invert the health check status, and select which regions the healthcheck should run from.
- Can monitor endpoint, cloudwatch alarm, or do a calculated healthcheck.

### Failover Routing Policy

- Failover to secondary site when healthcheck fails.
- Can only have one primary, and one secondary record (duh).

### Geo Location Routing Policy

- Routed based on user location.
- Need to have a default policy for users in a location that doesn't have an explicit policy.

### Multi-value Routing Policy

- Route traffic to multiple resources
- Each multi-value record is associated with a route53 health check.
- Up to 8 healthy records returned per multi-value query.
- If one resource becomes unavailable, it will be removed from the returned DNS record, and the client can chose one of the remaining available resources.
