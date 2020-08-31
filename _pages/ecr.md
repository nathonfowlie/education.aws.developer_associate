---
title: "ECR"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /ecr/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## Overview

- Private docker registry.
- Access controlled via IAM policy.
- Can enable tag immutability to prevent tags being over-written.
- Can scan images the moment that they're pushed to the registry.

## CLI authentication

### AWS CLI v1

The command output is executed, so need to wrap it as a shell command -

    $(aws ecr get-login --no-include-email --region <region>)

### AWS CLI v2

Uses pipes to do a docker login -

    aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <registry>

##  Docker Push/Pull

Same as usual.
