# Auto-Scaling Groups

## Overview

ASGs allow EC2 instances to be scaled out/in to match load. New instances will be automatically
registered with a load balancer and inherit the IAM roles that are attached to the ASG.

- A min/max EC2 instance count can be set to control costs/capacity.
- Use launch templates to manage ASGs. Avoid legacy launch configurations.
- An ASG can only be updated by providing a new launch configuration/template.
- When EC2 instances are terminated, the ASG will automatically create new instances to maintain capacity.
- EC2 Instances can be configured to terminate when a load balancer marks them as unhealthy.
- ASGs are free.

### Auto-Scaling Group Attributes

- Launch configuration.
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

### Auto-Scaling Alarms

ASGs can be scaled based on CloudWatch alarms, where the alarm can be anything - average CPU etc.

### Auto-scaling Rules

Auto-scaling can be triggered according to various rules - 

- Target average CPU usage.
- Number of requests on the ELB per instance.
- Average network in or out.
- Custom metrics send to CloudWatch from the application (connected user count etc).
- Schedule (usage patterns).

## Configuration

The ASG can use the launch template that's assigned to it, or you can specify the mix of instance
types and purchase options.

For example, keep a percentage of reserved instances, and auto-scale with On-Demand/Spot instances during peaks.

## Health Checks

There's two types of health checks available -

1. EC2 - Automatically terminate any unhealthy instances and provision replacements.
2. ELB - Automatically terminate any instances flagged as unhealthy by a load balancer, and provision replacements.

## Scaling

### Scaling Policies

#### Target Tracking Scaling

Simplest and easiest to setup.

???+ example "Example"
    I want the average ASG CPU to stay at around 40%

#### Simple/Step Scaling

Based on CloudWatch alarms.

???+ example "Example"
    When a CloudWatch alarm is triggered, scale out (ex: CPU > 70%).

#### Scheduled Actions

Anticipate scaling based on usage patterns.

???+ example "Example"
    Increase min capacity at 5pm on Fridays.

### Scaling Cool-down

Scaling cooldown helps ensure that the ASG doesn't launch/terminate instances before the next 
scaling event takes place.

They're usually used to manage scale in policies, which terminate instances based on a condition.
This gives EC2 auto-scaling more time to determine if it needs to terminated additional instances.

- You can create cool-down periods that apply to a specific simple scaling policy.
- A scaling specific cool-down overrides the default cooldown period.
- Scaling cool-down can be used to scale in faster to help reduce costs.
- Modify cool-down timers and the CloudWatch alarm period if the application is scaling in and out
  too many times each hour.
- The default cool-down period is 300 seconds.
- You can disable scale-in in the scaling policy, to make it scale-out only.
