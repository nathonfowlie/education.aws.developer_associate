# EC2

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
- AMIs are bound to a specific region, but can be copied across regions.

### Why use a Custom AMI?

- Pre-install required packages.
- Faster boot time by avoiding EC2 User Data.
- Pre-configure monitoring/enterprise software.
- Pre-install applications for faster deployments.
- Support Active Directory out of the box.
- Security concerns (need more control of the image).
- Control the maintenance & updates of AMIs over time.
- Optimised for a specific purpose.

### Building AMIs

- Start an EC2 instance, customise, stop it, and create an AMI using the stopped instance.

## Instance Types

| Type                  | Use Case                                                                                                               |
|-----------------------|------------------------------------------------------------------------------------------------------------------------|
| General Purpose       | Balance between compute, memory & networking. Web servers, code repos etc.                                             |
| Compute Optimised     | Compute intensive workloads. Batch jobs, transcoding, machine learning, gaming servers etc.                            |
| Memory Optimised      | Workloads processing large datasets. Databases, web cache stores, BI, realtime processing of unstructured data.        |
| Storage Optimised     | Workloads needing high IO. Databases, transaction processing, redis cache, distributed file systems, data warehousing. |

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
- Contain ALLOW rules only.
- Timeout errors mean it's a security group problem. Connection refused means it's an application error/EC2 instance isn't launched.

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
- Zonal reserved instance is bound to an AZ and can be used for capacity reservations.
- Regional reserved instances don't provide capacity reservations.
- Five main attributes - RAM, CPU, Memory, IO, Network, GPU.
- The letter represents the launch types specialty. C = cpu optimised, R = memory optmised etc.
- M instance types are all-rounders. Medium performance.
- T2/T3 instance types are burstable, based on burst credits.
  - Burst credits accumulate over time.
  - Once burst credits are exhausted, performance will absolutely tank.
- T2 has unlimited burst credits, but it costs money if the burst credit limit is exceeded.

| Launch Type          | Description | Good For | Committment |
|----------------------|-------------|----------|-------------|
| On Demmand           | Predictable pricing.<br/><br/>Billed by the second, with a minimum of 60seconds.<br/><br/>- No upfront payment.<br/><br/>- Highest hourly cost of all the instance launch types. | Short, un-interrupted workloads | None |
| Reserved             | Upto 75% cheaper than On-Demand instances.<br/><br/>Pay upfront.<br/><br/>Must specify the specific instance type to be reserved (SKU). | Long workloads, or steady state applications (database etc). | 1 - 3 years |
| Convertible Reserved | Upto 54% cheaper than On-Demand instances.<br/><br/>Instance type can be changed.<br/><br/>No upfront payment. | Long workloads | 1 - 3 years |
| Scheduled Reserved   | | A workload needs to be performed within a specific time window (9am-5pm Mon-Fri etc). | 1 year |
| Spot | Spot instance is given to the higest bidder. If someone bids more, then you can lose the instance with zero notice.<br/><br/>Four types - Load balancing (web services), Flexible (batch, ci/cd), Big data (any size, any AZ) and Defined duration (1-6hrs)<br/><br/>90% cheaper than On-Demand instances. | Short workloads - batch jobs, data analysis, image processing, things that are resilient to failure. |
| Dedicated Instance   | Runs on hardware that is dedicated to a specific account.<br/><br/>Hardware may be shared with other EC2 instances within the same account.<br/><br/>Can only move hardware after the EC2 instance is stopped.<br/><br/>Pay upfront.<br/><br/>Per instance billing. | | None |
  | Dedicated Hosts    | Reserve a physical server<br/><br/>Per instance billing<br/><br/>Provides visibility of the underlying sockets and hardware, such as physical cores.<br/><br/>User has full control over the instance placement.<br/><br/>Expensive | Companies with strong compliance/regulatory requirements, or for bring-your-own-license models where the license is bound to the number of cores. | 3 years |

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

## EC2 Instance Metadata

- Allows EC2 instances to learn about themselves without needing to use an IAM role.
- Accessed via ```HTTP GET http://169.254.169.254/[latest|<version>]/[dynamic|meta-data|user-data]```.
- Can retrieve the IAM role name, but not the policy.

## EC2 Instance Store

- Instance stores are ephemeral storage.
- Physically attached to the machine.
- Provides better IO performance (up to millions on IOPS).
- Good for buffering/caching/scratch data/temporary content.. Data survives reboots.
- Instance store is lost on termination and can't be resized. Backups need to be operated by the user.
- Up to 7.5TB, striped them to reach 30TB.
- Viewed on the instance as block storage.
- Risk of data loss if hardware fails.
- Some instances don't come with root EBS volumes.