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
