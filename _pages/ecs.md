---
title: "ECS"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /ecs/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## ECS Clusters

- Logical grouping of EC2 instances.
- Each EC2 instance runs the ECS agent (docker container).
- Each ECS agent registers itself to the ECS cluster.
- EC2 instances use a special AMI made specifically for ECS.
- 3 cluster creation templates
  - Networking only (use Fargate)
  - EC2 Linux + Networking (uses auto-scaling)
  - EC2 Windows + Networking (uses auto-scaling)
  
  
## ECS Task Definition

- JSON metadata that tells ECS how to run a docker container.
- Includes image name, port binding for container/host, memory/cpu requirements, env vars, network info + more.
- Need to assign an IAM role to allow it to make API requests to AWS services.
- Are versioned.


## ECS Service

- Defines how many/what tasks should be run.
- Can be linked to ELB/NLB/ALB if needed.
- Assign the task definition, and it's revision (version).
- Set minimum healthy percent to 0 to allow for rolling restarts.
- Can use Code Deploy to deploy to ECS (blue/green deployment), as an alternative to a rolling deployment.
- Can use placement strategies to control where the tasks are placed.
- Supports integration with service discovery.
- Supports auto-scaling.
- Two service types - daemon good for monitoring services, or replica, for 'normal' clustered containers.
- Needs a Service IAM role to allow it to communicate with the API. Role can be auto-created at the time of service creation.

## ECS Service with Load Balancer

- Use ALB with dynamic port forwarding.
- Only specify the container port in the task definition to use a random port (bridge networking).
- Load balancer can only be added during service creation.
- Have to create the load balancer before configuring the service.
- The security group used by the clusters EC2 instances, needs an inbound rule that permits all traffic on any port, coming from the ALB security group. This will allow the load balancer to forward traffic via the dynamically mapped ports.

### ALB

- Uses dynamic port forwarding to forward to the task instances running in swarm mode.
- Rule-based routing & paths supported.
- Define the listener port, protocol, target group name, target protocol (http, https etc), path (/ to handle all requests for all urls), and health check path.

### NLB

- Uses flow hashing route algorithm to direct traffic to one of the task instances.
- Uses the target group defined in the NLB.

### Classic Load Balancer

- Uses statically defined host port mappings. One task per container instance.
- Rule-based routing or paths not supported.
