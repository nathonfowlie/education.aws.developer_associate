---
title: "Elastic Load Balancer"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /elb/
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
sidebar:
  nav: "docs"
---

## What is EC2 Load Balancer (ELB)?

- Guaranteed to be working.
- AWS manages upgrades, maintenance, high availability.
- More expensive than setting up your own LB, but much less effort.
- Integrated with alot of AWS services.
- Use newer gen ELBs (more features).
- Can be external (public) or internal (private).
- Can auto-scale, but it's not instantaneous. Can contact AWS for a 'warm-up'.
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

### Health Checks

- Inform LB if instance is healthy.
- A Classic Load Balancer (v1) will perform the health check against a route & port (foo:8080/healthcheck).

## Stickyness

- Same client is always redirected to the same instance behind a load balancer.
- Uses a cookie with an expiration date to track which instance the client should be routed to.
- Helps to preserve session data.
- May cause a load imbalance.

## ELB Types

### Classic Load Balancer (v1)

- Supports TCP (Layer 4) and HTTP & HTTPS (Layer 7).
- Health checks are TCP or HTTP based.
- Have a fixed hostname <foo>.<region>.elb.amazonaws.com.
- One SSL certificate, doesn't support SNI.
  
### Application Load Balancer (v2)

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
- Supports SNI, using multiple listeners with multiple SSL certs.

#### ALB Target Groups

- EC2 instances (managed via auto-scaling group) - HTTP.
- ECS tasks (managed by ECS) - HTTP.
- Lambda functions - HTTP request translated into a JSON event.
- IP Addresses - Must be private IPs.
- Route to multiple target groups.
- Health checks performed at the target group level.
- Fixed hostname.
- Application servers don't see the client IP. Client ip/port/protocol are available via the X-Forwarded-For and X-Forwarded-Proto/X-Forwarded-Port headers.

### Network Load Balancer (NLB)

- TCP/UDP/TLS traffic (Layer 4).
- Handle millions of TPS.
- Less latency (100ms vs 400mb of ALB).
- One static IP per AZ.
- Supports assigned Elastic IPs.
- Used for extreme performance, and TCP/UDP level traffic.
- Security group doesn't see the traffic as coming from the NLB, which impacts the "source" attribute of the security groups rule.
- Subnets can't be disabled after the NLB is created, but new ones can be added. This means Elastic IP has to be created before the NLB is created (if you want to use an Elastic IP).
- User SNI to support multiple listeners, with multiple SSL certificates.

## ELB Security Groups

- Allow HTTP from <any> (public route)
- Allow HTTPS from <any> (public route)
- Allow HTTP from <security group 1 used by load balancer> (private route)
- Allow HTTPS from <security group 1 used by the load balancer> (private route)

## ELB Monitoring

- Access logs log all access requests.
- CloudWatch metrics provide aggregate stats (connections count etc).

### Troubleshooting ELB

- HTTP 503 means ELB is at capacity or there's no registered targets.
- If ELB can't connect to app, check security groups.

## ELB Connection Draining

- Called "connection draining" on CLB.
- Call "deregistration delay" on ALB and NLB.
- Is the time to complete in-flight requests while the instance is being de-registered or is unhealthy.
- Stops sending new requests to the instance which is de-registering.
- De-registration delay is 300sec by default. 
- De-registration delay can be anything from 1 to 3600 seconds.
- Default de-registration delay is 300 seconds.
- De-registration delay can be set to 0 (disabled).
- Use low value if requests are short.

## Cross-Zone Load Balancing

- Each load balancer instance distributes evenly across all registered instances in all AZs.
- Disabled by default on CLB. No additional charges for inter AZ data.
- Enabled on ALB, can't be turned off. No charges for inter AZ data.
- Disabled by default for NLB. Additional charges for inter AZ data.

## SLB/TLS Certificates

- Load balancers use an X.509 certificate.
- You can use/manage certificates from AWS Certificate Manager (ACM).
- You can use certificates from IAM.
- Can also use your own certificates.
- When configuring the HTTPS listener:
  - Must specify a default certificate.
  - An optional list of certs can be addded to support multiple domains.
  - Clients can use SNI (Server Name Indication) to specify the hostname they reach.
  - Can apply a security policy to support older versions of SSL/TLS.

### Server Name Indication (SNI)

- Use multiple SSL certificates on the same web server.
- Client indicates which hostname of the target server it should connect to during the SSL handshake.
- Only available on ALB, NLB and Cloudfront.
- Doesn't work on CLB.
