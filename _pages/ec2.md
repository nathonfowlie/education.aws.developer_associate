---
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /ec2/
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
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
| On Demmand  | Predictable pricing.<br/><br/>Billed by the second, with a minimum of 60seconds.<br/><br/>- No upfront payment.<br/><br/>- Highest hourly cost of all the instance launch types. | Short, un-interrupted workloads | None |
| Reserved | Upto 75% cheaper than On-Demand instances.<br/><br/>Pay upfront.<br/><br/>Must specify the specific instance type to be reserved (SKU). | Long workloads, or steady state applications (database etc). | 1 - 3 years |
| Convertible Reserved | Upto 54% cheaper than On-Demand instances.<br/><br/>Instance type can be changed.<br/><br/>No upfront payment. | Long workloads | 1 - 3 years |
| Scheduled Reserved | | A workload needs to be performed within a specific time window (9am-5pm Mon-Fri etc). | 1 year |
| Spot | Spot instance is given to the higest bidder. If someone bids more, then you can lose the instance with zero notice.<br/><br/>Four types - Load balancing (web services), Flexible (batch, ci/cd), Big data (any size, any AZ) and Defined duration (1-6hrs)<br/><br/>90% cheaper than On-Demand instances. | Short workloads - batch jobs, data analysis, image processing, things that are resilient to failure. |
| Dedicated Instance | Runs on hardware that is dedicated to a specific account.<br/><br/>Hardware may be shared with other EC2 instances within the same account.<br/><br/>Can only move hardware after the EC2 instance is stopped.<br/><br/>Pay upfront.<br/><br/>Per instance billing. | | None |
  | Dedicated Hosts | Reserve a physical server<br/><br/>Per instance billing<br/><br/>Provides visibility of the underlying sockets and hardware, such as physical cores.<br/><br/>User has full control over the instance placement.<br/><br/>Expensive | Companies with strong compliance/regulatory requirements, or for bring-your-own-license models where the license is bound to the number of cores. | 3 years |

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
