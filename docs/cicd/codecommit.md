# CodeCommit

## Overview

- Version control (git).
- Repos are private.
- No size limit.
- Integrated with Jenkins, CodeBuild, other CI tools.

## Security

### Authentication

- Standard git commands.
- Git auth is via SSH keys, or HTTPS.
- Supports MFA.
- No SSH option for the root user. Must use an IAM user.
- There's a maximum of 2 sets of HTTPS git credentials permitted per IAM user.

### Authorization

- IAM polices to manage user/roles access to repos.

### Encryption

- Encryption at rest using KMS.
- Encryption in transit (HTTPS or SSH).

### Cross Account Access

- Use IAM roles, and AWS STS (with the AssumeRole API).

## Notifications

- Can be triggered using AWS SNS, AWS Lambda, or AWS CloudWatch Event rules.

### Use Cases

| Service        | Use Cases                                                                                       |
|----------------|-------------------------------------------------------------------------------------------------|
| AWS SNS        | - Branch deletion<br/>- Trigger for push to master<br/>- Notify external build system.          |
| AWS Lambda     | - Branch deletion<br/>- Trigger for push to master<br/>- Notify external build system.          |
| AWS CloudWatch | - Trigger for pull requests<br/>- Commit comment events<br/>- Event rules go into an SNS topic. |
