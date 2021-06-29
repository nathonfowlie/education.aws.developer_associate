# CodeBuild

## Overview

- Fully managed build service.
- Only pay for the time to complete a build.
- Uses docker under the hood.
- Can be extended via custom docker images.
- Source code from CodeCommit, GitHub or Bitbucket.
- Build steps are defined in code (buildspec.yml in the vcs root). Can sit in CodeBuild, or CodePipeline.
- Build logs are stored in S3 or CLoudWatch logs.
- Can be re-producted locally to troubleshoot errors.
- Build metrics can be captured.
- S3 buckets can be used as a temporary cache for build dependencies to improve build times.
- Needs an IAM service role to get permission to perform actions against AWS API/resources.
- Provide a VPC configuration to access VPC resources from CodeBuild.

## Supported Environments

- Java
- Ruby
- Python
- Go
- Node.js
- Android
- .NET Core
- PHP
- Docker (whatever you want)

## Security

- KMS to encrypt artifacts.
- IAM for build permissions.
- VPC for network security.
- CloudTrail for API call auditing.
- Reference SSM parameters/Secrets Manager secrets in environment variables.

## Notifications

- CloudWatch alarms to detect failed builds & trigger notifications.
- CloudWatch Events/Lambda as a glue for notifications.
- Generate SNS notifications.

## BuildSpec

- buildspec.yml must be at the root of the code.

### BuildSpec Layout

- Define environment variables
  - Plain-text
  - Secrets (using SSM parameter store)
- Phases
  - Install: Resolve dependencies
  - Pre-build: Final prep before build execution.
  - Build: Build the code.
  - Post-build: Final build actions (zip build output etc)
- Artifacts
  - What to upload to S3 (encrypted with KMS)
- Cache
  - Files to be cached to S3 to improve build performance (eg: build dependencies)

## Local Build

- Uses docker.
- Need to install the CodeBuild Agent.

## CodeBuild in VPCs

- By default, containers are built outside of the VPC, and can't access VPC resources.
- Specify a VPC ID, subnet ids, security group ids etc in order to access them.
- Public VPCs can't (shouldn't) have an internet connection. Use a private subnet with 0.0.0.0/0 destination as the target NAT gateway.
