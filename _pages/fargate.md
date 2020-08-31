---
title: "Fargate"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /fargate/
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
sidebar:
  nav: "docs"
---

## Overview

- Serverless way to run containers. AWS will assign an ENI to the containers. No EC2 instances to provision/manage.
- Just create Fargate compatible task definitions and AWS will figure the rest out.
- Can assign IAM roles to Fargate tasks to execute actions against AWS.
- Doesn't need explicitly defined host port mapping.
