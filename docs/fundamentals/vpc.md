# VPC

##  What is a VPC?

- Private network to deploy resources.
- VPC is bound to a region.

## Main Components

### Subnets

- Partition network inside VPC.
- Bound to the availability zone.
- Public subnet can be accessed from the internet.
- Private subnet is only accessible from within the VPC.

### Route Tables

- 

### Internet Gateway

- Allows VPC instances to connect to the internet.
- Public subnet has a route to the IGW.

### NAT Gateways/Instances

- NAT gateways are managed by AWS.
- NAT instances are self-managed.
- Allow VPC instances on private subnets to acess the internet, while remaining private.

### Network ACL

- Firewall to control inbound/outbound subnet traffic.
- Rules only include IP addresses.
- Operates at the subnet level.
- Supports ALLOW and DENY rules.
- Is stateless. Return traffic must be explicitly allowed by rules.
- Rules are processed in number order.
- Automatically applies to all instances in the subnet its associated with.

### Security Groups

- Firewall to control inbound/outbound EC2/ENI traffic.
- Rules can use IP addresses as well as other resources.
- Operates at the ENI/EC2 instance level.
- Only supports ALLOW rules (not DENY).
- Is stateful, return traffic is automatically allowed.
- All rules are evaluated before allowing/blocking the traffic.
- Applies to instances only if they've been assigned the security group.

## VPC Flow Logs

- Captures information about IP traffic
  - VPC flow logs
  - Subnet flow logs
  - ENI flow logs
- Useful for troubleshooting connectivity issues.
- Captures network information from any AWS managed interface
  - ELBs
  - Elasicache
  - RDS
  - Audora etc
 - VPC flow log data can be sent to S3/CloudWatch logs.

## VPC Peering

- Connect two VPCs privately using the AWS network.
- Makes VPCs behave as though they're in the same network.
- Must not have overlapping CIDRs.
- Peering connection is non-transitive. If VPC A and B have peering setup,and VPC B & C have peering setup, VPC C can't reach A via B.

## VPC Endpoints

- Connect to AWS service using a private network instead of public www network.
- Improves security and reduces latency to access AWS services.
- Only used within the VPC.

### VPC Endpoint Gateway

- For S3 and DynamoDB.
- Allow private resources to access S3/DynamoDB using private AWS network.

### VPC Endpoint Interface (ENI)

- For any other AWS service.

## Site to Site VPN

- Connect an on-prem VPN to AWS.
- Uses the public internet.
- Comms are encrypted.
- Can't access VPC endpoints.

## Direct Connect

- Physical connection to AWS.
- Private network.
- Requires at least 1month to setup.
- Can't access VPC endpoints.
