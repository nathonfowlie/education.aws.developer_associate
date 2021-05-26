  # Elastic File System

## What is Elastic File System (EFS)?

- Managed NFS volumes that can be mounted on many EC2 instances, across many AZs.
- Very expensive.
- Pay based on what is used (automatic scaling).
- Highly available.
- Good for content management, web serving, data sharind, wordpress.
- Uses NFSv4.1.
- Uses security groups to control access.
- Only compatible with linux.
- Can use lifecycle management to remove files after a number of days.
- Throughput mode has two options
  - Bursting
  - Provisioned
- Performance mode has two options
  - General purpose
  - Max I/O
- Can have a public or private IP.
- Two modes
  - Standard: Frequently accessed files.
  - Infrequent Access (EFS-IA): cheaper to store files, but additional cost to retrieve files.

## Mounting EFS volumes

- Need to install the amazon-efs-utils package or nfs-utils.
- Mounting using efs helper, or nfs client.
- Make sure security group allows NFS port 2049.

## Cleaning up EFS

- Make sure volumes are removed after deleting EFS volume.
- Make sure security group(s) are cleaned up.

## EBS vs EFS

- EBS can only be attached to one instance at a time. EFS can attach to multiple instances.
- EBS is bound to a specific AZ, EFS is multi-AZ.
- EFS only for linux (NFS).
- EFS is more expensive than EBS.
- Can use EFS-IA to save money via lifecycle policy.
- EFS billed for what you use. EBS you pay for provisioned capacity.
- gp2 IO increases if the disk size increases.
- io1 can increase IO independantly of disk size.
- To migrate EBS volume across AZ, take a snapshot, and restore into the new AZ. Uses alot of IO.
- EBS will be terminated by default when the instance is terminated (can be turned off).

## Encryption

- Encryption at rest using KMS keys.

## Scaling

- Supports thousands of concurrent clients.
- Upto 10GB/sec throughput.
- Can grow to PB scale file systems.
- Performance mode is set at creation time.

### Performance Mode

#### General Purpose (default)

- Use for latency sensitive use cases (web servers etc).

#### Max I/O

- Higher latency.
- Use for highly parallel workloads (big data, media processing).

### Throughput Mode

#### Bursting

- 50MB/sec per TB, up to 100MB/sec bursts.

#### Provisioned

- Define throughput seperately from storage size.
- Max 1GB/sec throughput.

## Storage Tiers

| Tier                   | Use Case                                      |
|------------------------|-----------------------------------------------|
| Standard               | For frequently accessed files.                |
| Infrequent Access (IA) | Cost to retrieve files, lower price to store. |

