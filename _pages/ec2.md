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

## Security Groups

- Provide access to ports.
- Control authorised port ranges.
- Control inbound/outbound traffic.
- Can attach to multiple instances.
- EC2 instance can have multiple security groups.
- Bound to a specific Region/VPC combination.
- Reside outside of the EC2 instance so the instance never recieves blocked traffic.
- All inbound traffic is blocked by default.
- All outbound traffic is permitted by default.
- Security groups can reference another security group for defining inbound/outbound rules (SG1 is allowed to connect to EC2 instances assigned SG2, but SG3 isn't).
- Generally a good idea to keep a seperate SSH security group as it's commonly required.

## EC2 User Data

- Runs once, the first time an instance is started.
- Runs as root.
- Script is base64 encoded.

## EC Instance Connect

- Provides web-based SSH access to EC2 instances.

## Elastic Network Interfaces

- Virtual NIC.
- Has 1 primary private IPv4 address, and one or more secondary IPv4 addresses.
- Gets 1 Elastic IPv4 address per private IP.
- Can have 1 public IPv4 address.
- Can have multiple security groups.
- Is assigned a Mac address like a physical NIC.
- Bound to a specific Availability Zone.
- Can be created independently and moved between EC2 instances on the fly to support failover.

## Elastic IPs

- Assigned to an EC2 instance, so that it's IP doesn't change when the instance is stopped, then started.
- 1 public IPv4 is assigned when the instance is spun up.
- Elastic IP changes whenever the instance is stopped, then started. The private IP is reverved for the life of the instance.
- Maximum of 5 elastic IPs per account (but can be extended).
- Should be avoided as much as possible.
- Costs money when not in use.

## EC2 Instance Launch Types

- Use reserved instance type for base application, and On-Demand/Spot instance types to handle peaks.
- Five main attributes - RAM, CPU, Memory, IO, Network, GPU.
- There's over 50 of the damned things.
- The letter represents the launch types specialty. C = cpu optimised, R = memory optmised etc.
- M instance types are all-rounders. Medium performance.
- T2/T3 instance types are burstable, based on burst credits.
  - Burst credits accumulate over time.
  - Once burst credits are exhausted, performance will absolutely tank.
- T2 has unlimited burst credits, but it costs money if the burst credit limit is exceeded.

| Launch Type | Description | Good For | Committment |
|-------------|-------------|----------|-------------|
| On Demmand  | <ul><li>Predictable pricing.</li><li>Billed by the second, with a minimum of 60seconds.</li><li>No upfront payment.</li><li>Highest hourly cost of all the instance launch types.</li></ul> | Short, un-interrupted workloads | None |


### On Demand Instance

- Good for short, un-interrupted workloads.
- Have predictable pricing.
- Billed by the second, with a minimum of 60seconds.
- No upfront payment.
- No long-term committment.
- Highest hourly cost of all the instance launch types.

### Reserved Instances

- Minimum reservation is 1 year, maximum is 3 years.
- Upto 75% cheaper than On-Demand instances.
- Best for long workloads, or steady state applications (database etc).
- Pay upfront.
- Must specify the specific instance type to be reserved (SKU).

#### Convertible Reserved Instances

- Upto 54% cheaper than On-Demand instances.
- Instance type can be changed.
- Best for long workloads.
- No upfront payment.

#### Scheduled Reserved Instances

- Reserved in 12month blocks.
- Best for when a workload needs to be performed within a specific time window (9am-5pm Mon-Fri etc).

### Spot Instances

- Best for short workloads - batch jobs, data analysis, image processing, things that are resilient to failure.
- Spot instance is given to the higest bidder. If someone bids more, then you can lose the instance with zero notice.
- There are 4 different types of workloads that can be requested:
  - Load Balancing (Web services).
  - Flexible (Batch jobs, CI/CD).
  - Big Data (Instance can be of any size, in any Availability Zone).
  - Defined Duration (Between 1 and 6 hrs).
- 90% cheaper than On-Demand instances.

### Dedicated Instances

- Runs on hardware that is dedicated to a specific account.
- Hardware may be shared with other EC2 instances within the same account.
- Can only move hardware after the EC2 instance is stopped.
- Pay upfront.
- Per instance billing.

### Dedicated Hosts

- Book a physical server.
- Uses per instance billing.
- Good for companies with strong compliance/regulatory requirements, or for bring-your-own-license models where the license is bound to the number of cores.
- Reserved for 3 years.
- Provides visibility of the underlying sockets and hardware, such as physical cores.
- User has full control over the instance placement.
- Very expensive option (duh?).

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
