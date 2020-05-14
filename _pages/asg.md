---
permalink: /asg/
toc: true
toc_label: "Table of Contents"
toc_icon: "book-reader"
sidebar:
  nav: "docs"
---

## What's an Auto-scaling Group?

- Scale out/in EC2 instances to match load.
- Set minimum/maximum number of machines.
- Automatically registeers new instances with a load balancer.
- Launch templates are the newer version of launch configurations and recommended way to manage ASGs.
- To update an ASG, a new launch configuration/template has to be provided.
- IAM roles attached to an ASG will be assigned to EC2 instances.
- ASGs are free.
- When instances are terminated, the ASG will automatically create new instances.
  - Instances can be terminated when marked as unhealthy by a LB.

### ASG Attributes

- Launch configuration
  - AMI + instance type.
  - EC2 User data.
  - EBS volumes.
  - Security groups.
  - SSH key pair.
  - Min/Max size.
  - Initial/desired capacity.
  - Network/subnet info.
  - Load balancer information.
  - Scaling policies.

### Auto-scaling Alarms

- ASG can be scaled based on CloudWatch alarms.
- Alarms can be anything, average CPU etc.

### Auto-scaling  Rules

- Target average CPU usage.
- Number of requets on the ELB per instance.
- Average network in or out.
- Custom metrics send to CloudWatch from the application (connected user count etc).
- Schedule (usage patterns).

### 
