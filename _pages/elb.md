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

## What is Load Balancing?

- Forward traffic to multiple servers (EC2 instances).

## Why use a Load Balancer?

- Spread load.
- Expose a single endpoint (DNS) to the application.
- Seamlessly handle downstream failures.
- Perform regular health checks of instances.
- SSL termination.
- Manage sticky sessions.
- HA across AZs.
- Segregate public and private traffic.

## EC2 Load Balancer (ELB)

- Guaranteed to be working.
- AWS manages upgrades, maintenance, high availability.
- More expensive than setting up your own LB, but much less effort.
- Integrated with alot of AWS services.
- Use newer gen ELBs (more features).
- Can be external (public) or internal (private).
- Can auto-scale, but it's not instantaneous. Can contact AWS for a 'warm-up'.

### ELB Types

#### Classic Load Balancer (v1)

- Supports TCP (Layer 4) and HTTP & HTTPS (Layer 7).
- Health checks are TCP or HTTP based.
- Have a fixed hostname <foo>.<region>.elb.amazonaws.com.
  
#### Application Load Balancer (v2)

- Supports HTTP, HTTPS and WebSockets (Layer 7).
- Load balance multiple HTTP applications across machines (target groups).
- Load balance multiple HTTP applications on the same machine (containers).
- Supports HTTP/2 and WebSocket.
- Supports redirects.
- Supporting routing tables based on target groups:
  - Path in URL.
  - Hostname in URL.
  - Query string & headers.
- Good for micros services and container-based applications.
- Supports port mapping to redirect to dynamic ports (useful for ECS).
- Would require multiple CLBs to load balance multiple applications, vs 1 ALB.

##### ALB Target Groups

- EC2 instances (managed via auto-scaling group) - HTTP.
- ECS tasks (managed by ECS) - HTTP.
- Lambda functions - HTTP request translated into a JSON event.
- IP Addresses - Must be private IPs.
- Route to multiple target groups.
- Health checks performed at the target group level.
- Fixed hostname.
- Application servers don't see the client IP. Client ip/port/protocol are available via the X-Forwarded-For and X-Forwarded-Proto/X-Forwarded-Port headers.

#### Network Load Balancer (v2)

- TCP
- TLS
- UDP

### Health Checks

- Inform LB if instance is healthy.
- A Classic Load Balancer (v1) will perform the health check against a route & port (foo:8080/healthcheck).

### ELB Security Groups

- Allow HTTP from <any> (public route)
- Allow HTTPS from <any> (public route)
- Allow HTTP from <security group 1 used by load balancer> (private route)
- Allow HTTPS from <security group 1 used by the load balancer> (private route)

### ELB Monitoring

- Access logs log all access requests.
- CloudWatch metrics provide aggregate stats (connections count etc).

### Troubleshooting ELB

- HTTP 503 means ELB is at capacity or there's no registered targets.
- If ELB can't connect to app, check security groups.
