---
title: "AWS Certificate Manager (ACM)"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /acm/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## Overview

- Host public SSL certificates in AWS.
  - BYO and upload using the CLI or
  - Have ACM provision and renew the SSL certs (free).
- Can manage private certificates as well.

## Integration

Can load certs onto:

- Load Balancers (including ones created by Elastic Beanstalk).
- Cloudfront distributions.
- APIs on API gateways.
