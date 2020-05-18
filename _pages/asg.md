---
header:
  caption: Auto-scaling Groups (ASG)
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /asg/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
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


### Configuring ASG

- Can adhere to the launch template assigned to the ASG, or specify the mix of instance types and purchase options (ie: keep a percentage of reserved instances, and auto-scale with On-Demand/Spot instances during peaks).
- Two types of health checks available when setting up the ASG. EC2, or ELB. Select ELB to have the ASG automatically terminate any unhealthy instances and provision new ones.

### Scaling Policies

- Target tracking scaling
  - Most simple and easiest to setup.
  - I want the average ASG CPU to stay at around 40%
- Simple/Step Scaling
  - When a CloudWatch alarm is triggered, scale out (ex: CPU > 70%).
  - When a CloudWatch alarm is triggered, scale in (ex: CPU < 30%).
- Scheduled Actions
  - Anticipate scaling based on usage patterns (increase min capacity at 5pm on Fridays).

### Scaling Cool-down

- Helps ensure ASG doesn't launch/terminate instances before the next scaling event takes place.
- Can create cool-down periods that apply to a specific simple scaling policy.
- A scaling specific cool-down overrides the default cooldown period.
- Commonly used for scale-in policies. Onces that terminate instances based on criteria. Gives EC2 auto-scaling more time to determine if it needs to terminated additional instances.
- Scaling cool-down can be used to scale in faster to help reduce costs.
- Modify cool-down timers and the CloudWatch alarm period if the application is scaling in and out too many times each hour.
- Default cool-down period is 300 seconds.
- Can disable scale-in in the scaling policy, to make it scale-out only.
