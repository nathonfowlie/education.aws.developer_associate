# CodeDeploy

## Overview

- Each EC2/On-Prem machine needs to be running the CodeDeploy agent.
- Agents poll CodeDeploy for changes.
- CodeDeploy sends the appspec.yml file to the agent.
- The application is pulled from GitHub or S3.
- EC2 will run deployment instructions
- CodeDeploy Agent reports the success/failure of the deployment.
- EC2 instances are grouped by deployment group (dev, test, prod etc).
- Can integrate with CodePipeline, and use CodePipeline artifacts.
- Can use existing tools, applications.
- Integrated with auto-scaling.
- Supports Blue/Green deployments (EC2 only).
- Supports Lambda deployments.
- Doesn't provision resources.

## Main Components

- Application: Unique name
- Compute platform: EC2, On-Premise (hosts need to be tagged) or Lambda
- Deployment Configuration: Deployment rules for success/failure
- Deployment Group: group tagged instances.
- Deployment Type: In-place, or Blue/Green.
- IAM instance profile: To give EC2 permissions to pull from S3 or GitHub.
- Application Revision: app code + appspec.yml file.
- Service Role: Role for CodeDeply to perform what it needs.
- Target Revision: Target application version.

## AppSpec.yml

- File section: Where to get the source files from S3/GitHub.
- Hooks: Instructions on how to deploy the new version (have timeouts). The order is:
  - ApplicationStop
  - DownloadBundle
  - BeforeInstall
  - AfterInstall
  - ApplicationStart
  - ValidateService
  - BeforeAllowTraffic
  - AllowTraffic
  - AfterAllowTraffic

### Example

    version: 0.0
    os: linux
    files:
      - source: /index.html
        destination: /var/www/html/
    hooks:
      BeforeInstall:
        - location: scripts/install_dependencies
          timeout: 300
          runas: root
        - location: scripts/start_server
          timeout: 300
          runsas: root
      ApplicationStop:
        - location: scripts/stop_server
          timeout: 300
          runsas: root

## Deployment Configuration

- One instance at a time. Stop if an instance fails.
- Deploy to 50% of instances.
- All at once. Good for dev.
- Custom: min healthy hosts etc.

## Deployment Failures

- Instances stay in "Failed" state.
- New deployments will be deployed to failed state instances first.
- Rollback by re-deploying the old deployment or enable automated rollback.

## Deployment Targets

- Set of EC2 instances with tags
- Directly to an ASG
- Mix of ASG/tags to build deployment segments.
- Customised scripts with DEPLOYMENT_GROUP_NAME environment variables (if in dev, foo, else if in prod, bar).

## EC2

- Define how to deploy the application using appspec.yml
- Will do in-place updates/blue-green deployment to EC2 instances.


## ASG

### In-Place Updates

- Update the existing EC2 instances.
- New instances created by an ASG will also get the automated deployments.

### Blue/Green Deployments

- A new ASG will be created with identical settings to the existing ASG. 
- Can choose how long to keep the old instances.
- Must be using an ELB.


## Rollbacks

- Can specify automated rollbacks.
  - When deployment fails.
  - When CloudWatch alarm is triggered.
  - Disable rollbacks completely.
- The last known good revision is re-deployed as a new deployment, with a new version id.
