---
title: "CloudFront"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /cloudfront/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## Overview

- Content Delivery Network (CDN).
- Improves read performance, content cached at the edge.
- 216 edge locations globally.
- DDos protection
- Integration with Shield, Web Application Firewall.
- Can expose external HTTPS and talk to internal HTTPS backends.

## Origins

### S3 Bucket

- Distribute files globally & cache them at the edge.
- Better security via CloudFront Origin Access Identity (OAI).
- Used as an ingress to upload S3 files.
- Uses OAI + S3 Bucket policy.

### Custom (HTTP

- ALB (must be public, and SG must allow public IP of edge locations)
- EC2 instance (must be public, and SG must allow public IP of edge locations)
- S3 Website
- Any HTTP backend

## High-level Flow

Client => HTTP GET /foo.jpg => Edge Location => S3 Bucket/HTTP (if not cached) => Edge Location (add/update cache) => Client

## Geo Restriction

- Restrict who can access the distribution based on country.

## CloudFront vs S3 CRR

- CloudFront uses a global edge network and files are cached for a TTL (default is 24hrs). Useful for static content.
- S3 CRR needs to be setup in each region replication needs to happen.  Files are uploaded in near realtime, it's read-only and better suited for dynamic content that needs low latency.
