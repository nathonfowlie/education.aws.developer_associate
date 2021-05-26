# EBS

## What's an EBS Volume?

- EBS is a network attached volume to persist data.
- Can be de-ttached and re-attached to another EC2 instance in the same AZ.
- Bound to an AZ.
- Capacity must be provisioned in advance (IOPS, and volume size).
- Billed for all provisioned capacity, even if it's not used.
- Capacity can be increased as needed.
- Only GP2 and IO1 can be used as boot volumes.
- Enable "Delete on Termination" to delete the EBS volume when the EC2 instance is terminated. By default it's enabled on the root volume.

## EBS Attributes

- Size
- Throughput
- IOPS

## EBS Volume Types

| Volume Type     | Optimised for                 | Boot Volume | Uses                                                                                                                                  | Size       | IOPS                                                                       |
|-------------|-------------------------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------|------------|----------------------------------------------------------------------------|
| GP2 (SSD)   | Balance price vs performance. | Yes         | General purpose.<br/>Low latency interactive apps<br/>Virtual desktops<br/>Dev/Test environments<br/>Recommended for most workloads.  | 1GB-16TB   | 3 IOPS/GB<br/>Minimum 100 IOPS with burst to 3000. Maximum 16,000 IOPS.<br/>IOPS scales with volume size.    |
| GP3 (SSD)   | Balance price vs performance. | Yes         | General purpose.<br/>Low latency interactive apps<br/>Virtual desktops<br/>Dev/Test environments<br/>Recommended for most workloads.  | 1GB-16TB   | Minimum 3,000 IOPS and 125MB/s. Maximum 16,000 IOPS and/or 1000MB/s.    |
| IO1/IO2 (SSD)   | High performance SSD          | Yes         | Large workloads & critical business applications - DB servers etc.<br/>Highest performance SSD.<br/>Expensive.<br/>IO2 should be preferred over IO1 because it gets more IOPS/GB, and more durability. | 4GB-16TB   | 50IOPS/GB<br/>Minimum 100 IOPS, upto 32,000. Or 64,00 with Nitro instance.<br/>Supports EBS multi-attach. |
| IO2 Block Express (SSD)   | High performance SSD          | Yes         | Large workloads & critical business applications - DB servers etc.<br/>Highest performance SSD.<br/>Expensive.<br/>Sub millisecond latency.<br/>Supports EBS multi-attach. | 4GB-64TB   | 1,000IOPS/GB, upto 256,000 IOPS |
| ST1 (HDD)       | Frequently accessed, throughput intensive workloads. | No          | Kafka<br/>Data warehouses<br/>Log processing<br/>Low cost. | 125MB-16TB | Maximum 500 IOPS<br/>Maximum 500MB/sec through-put.                        |
| SC1 (HDD)   | Infrequently accessed data        | No          | Cheapest. Good for cold backups, snapshots. | 125MB-16TB | Maximum 250MB/sec throughput with 250 IOPS.                                              |

## EBS Snapshots

- Can be copied across regions/AZs.
- Don't necessarily have to detach the volume to take a snapshot.
- Can create a new volume from a snapshot.

## EBS Multi Attach

- Attach same EBS volume to multiple EC2 instances in the same AZ.
- Good for clustered applications (eg: teradata).
- Must use a cluster aware file system.
- Only for IO1 or IO2.
