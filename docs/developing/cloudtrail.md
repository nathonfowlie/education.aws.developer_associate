# CloudTrail

## Overview

- Governance, compliant and audit for the AWS account.
- Enabled by default.
- History of events/API calls.
- Can put logs from CloudTrail into CloudWatch.
- If a resource is deleted, look at CloudTrail first.
- Need to chose what log events to record.

## Log Events

| Event            | Description                                                               |
|------------------|---------------------------------------------------------------------------|
| Management Event | Management operations performed on AWS resources. Can exclude KMS events. |
| Data Event       | Resource operations performed on/within AWS resources (S3 or Lambda).     |
| Insights Event   | Unusual activity, errors, or user behaviour.                              |