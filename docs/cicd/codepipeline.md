# CodePipeline

## Overview

- Continuous delivery
- Visual workflow
- Pull source from CodeCommit, ECR, S3, or GitHub.
- Build using CodeBuild, Jenkins etc.
- Load test using 3rd party tools
- Deploy with CloudFormation, CodeDeploy, Beanstalk, Service Catalog, ECS, ECS (Blue/Green) or S3.
- Supports sequential and parallel actions.
- Supports manual approval.

## Action Group

- Defined within a stage. It's a build step.

## CodePipeline Artifacts

- Files output by each stage, for use in the next stage.
- Artifacts are stored in S3.
- S3 bucket can be created automatically, or explicitly defined.

## Change Detection

- CloudWatch events. This is the faster & preferred approach.
- Periodically poll CodePipeline for changes.

## Security

- Use a service role (IAM) to give the pipeline permissions to interact with AWS API/resources.

## Troubleshooting

- CodePipeline state changes trigger a CloudWatch event, which can create a SNS notification.
  - Failed pipelines.
  - Cancelled stages.
- Information about failed stages can be found through the console.
- AWS CloudTrail to audit AWS API calls.
- If the pipeline can't perform an action, check that the "IAM Service Role" has enough permissions in the IAM policy.
