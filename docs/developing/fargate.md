# Fargate

## Overview

- Serverless way to run containers. AWS will assign an ENI to the containers. No EC2 instances to provision/manage.
- Just create Fargate compatible task definitions and AWS will figure the rest out.
- Can assign IAM roles to Fargate tasks to execute actions against AWS.
- Doesn't need explicitly defined host port mapping.
