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

## Overview

Three different flavours -

| Flavour     | Description                                   |
|-------------|-----------------------------------------------|
| ECS Classic | Provision EC2 instances to run containers on. |
| Fargate     | Serverless.                                   |
| EKS         | Managed Kubernetes by AWS.                    |

### ECS Classic

- EC2 instances must be created.
- Must configure /etc/ecs/ecs.config with the cluster name.
- EC2 instances must run an ECS agent.
- EC2 instance can run multiple containers of the same type, by NOT specifying a host port and using an ALB with dynamic port mapping.
- EC2 instance needs to allow traffic from the ALB on all ports.
- ECS tasks must have IAM roles to execute actions against AWS.
- Security groups operate at the instance level, not the task level.

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

## IAM Roles

### AWS Service role for ECS

- Allow the ECS service to create/manage resources (setup load balancers, route53 etc).

### EC2 Instance Role

- Allow EC2 instances in an ECS cluster to access ECS (register with load balancers etc).
- Attached to EC2 instance(s).
- Allows the ECS agent to function correctly (make API calls to the ECS service, send container logs to CloudWatch, and pull images from ECR).

### ECS Service Role

- Allows ECS to create and manage AWS resources (setup load balancers, route53 etc).

### ECS Container Service Autoscaling Role

- Allow auto-scaling to access and update ECS services.

### ECS Service Task Role

- Controls what resources the task can/can't access.

## Task Placement & Constraints

- Only for ECS using EC2. Fargate will figure it out for you.
- Based on best effort only.
- Uses the workflow -
  1. Find instances that meet CPU, memory & port requirements in the task definition.
  2. Find instances that meet task placement constraints.
  3. Find instances that satisfy task placement strategies.
  4. Selet the instance for task placement.

### Task Placement Strategies

Multiple strategies can be mixed together. For example, spread the containers out across all availability zones, but binpack them together to maximum EC2 instance utilisation in each zone.

| strategy | description                                                                                                               |
|----------|---------------------------------------------------------------------------------------------------------------------------|
| binpack  | Places tasks based on the least available amount of CPU or memory, to maximise EC2 instance utilisation and reduce costs. |
| random   | Places tasks randomly.                                                                                                    |
| spread   | Place tasks based on a specified value - instance id, availability zone etc.                                              |

### Task Placement Contraints

| constraint       | description                                                                             |
|------------------|-----------------------------------------------------------------------------------------|
| distinctInstance | Place each task on a different container instance.                                      |
| memberOf         | Place task on instances that satisfy an expression, using Cluster Query Language (CQL). |

## Service Auto-scaling

- Different to EC2 auto-scaling.
- Fargate auto-scaling is way easier.

| Scaling method    | Description                               |
| TargetTracking    | Scale based on average CloudWatch metric. |
| Step Scaling      | Scale based on CloudWatch alarms.         |
| Scheduled Scaling | Scale based on predictable changes.       |

## Cluster Capacity Provider

- Will provision infrastructure for you (scale EC2 instances, ECS task instances and Fargate instances).
- The FARGATE and FARGATE_SPOT capacity providers are added automatically of ECS and Fargate users.
- For Amazon ECS on EC2, the capacity provider needs to be associated with an auto-scaling group.
