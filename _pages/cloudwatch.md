---
title: "CloudWatch"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /cloudwatch/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## Overview

- Provides metrics for every AWS service.
- Metrics are grouped by namespace.
- Dimensions are attributes of a metric (instance id, environment etc).
- Max 10 dimensions per metric.
- Metrics have timestamps.
- Alarms can trigger notifications for any metric.
- Alarms can be attached to Auto Scaling, EC2 actions, SNS notifications.
- Alarms can be based on sample, percentage, max, min etc).
- Alarms states are OK, INSUFFICIENT_DATA, ALARM.
- Alarm periods are the time window used to evaluate the metric.

## EC2 Detailed Monitoring

- EC2 instance metrics have 5min granularity by default. 
- Detailed monitoring sets granularity to 1min.
- 10 detailed monitoring metrics on the free tier.
- EC2 memory usage is not pushed by default. Needs to be a custom metric.
- Group metric collection is not enabled by default for EC2 instances.

## Custom Metrics

- 1min granularity.
- Enable high-resolution custom metrics (```StorageResolution``` API parameter) to set granularity up to 1second.
- High resolution metrics only allow 10sec or 30sec for the alarm period.
- Use ```PutMetricData``` to send a custom metric to CloudWatch.
- Use expontential backoff in case of throttle errors.
