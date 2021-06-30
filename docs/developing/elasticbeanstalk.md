# Elastic Beanstalk

## Overview

- Developer centric view of deploying apps on AWS.
- Uses EC2, ASG, ELB, RDS etc.
- Free, but pay for underlying resources.
- Handles instance & OS configuration.
- Provides a deployment strategy.
- Developer only responsible for application code.
- Has a rollback feature to rollback to previous application version.
- By default doesn't persist storage.
- Self-signed certificate used for HTTPS if a custom domain isn't purchased.
- Supports TCP pass-through.
- Store SSL private keys on S3 and download them from there.
- If doing HTTP to HTTPS redirect, ensure the health check can still run over HTTP.

### Three Architectural Models

- Single Instance Deployment: Good for development
- LB + ASG: Good for production or pre-production web applications.
- ASG Only: Good for non-web applications in production (workers etc)

### Three Components

#### Application

- Collection of Elastic Beanstalk components (environments, versions, environment configurations).

#### Application Version

- Tag that points to an S3 object containing the deployable code (war file etc).

#### Environment

- Collection of AWS resources running an application version.
- Elastic Beanstalk will provision the AWS resources needed to run the application version (EC2 instances, ASG etc).
- Use ```Environment Links``` to split components between multiple environments, and link them together to share information.
- Uses Route53 to generate the environments CNAME.
- Does SSL termination at the load balancer by default, but can be configured to use SSL all the way to the EC2 instance.
- Use ACM, or IAM to distribute SSL certificate.
- Add a HTTPS listener under .ebextensions/securelistener-[clb|alb|nlb].config.

##### Worker Environment Tier

- Uses SQS to pass off long-running tasks to a back-end.
- Scheduled tasks can be setup using cron.
- Failed tasks will be re-queued after the ```ErrorVisibilityTimeout``` period.
- Unresponsive tasks will be re-queued after the ```InactivityTimeout``` period.
- Supports Dead Letter Queues.
- Auto-scales via CloudWatch.
- Use ```MaxRetries``` to limit how many times the message is re-queued.

##### Web Environment Tier

- Front-end handling incoming requests (ie: web server).
- Requests come into the web environment tier, put tasks in an SQS queue, which will read the tasks from the
  SQS queue.

### Container Type

- Defines the infrastructure toplogy, and software stack running on the EC2 instances. eg: LAMP, Tomcat.

### Host Manager

- Runs on each EC2 instance.
- Deploy the application.
- Aggregates events/metrics.
- Monitors application logs for errors.
- Patches components.
- Handles log rotation and logging to S3.

### Environment Lifecycle

1. Create application.
2. Create environments.
3. Upload an application version (+ alias)
4. Release to environments.

![ELB Workflow](../images/elb_workflow.png)

### Supported Languages

- Go, Java, Tomcat, .NET, NOdeJS, PHP, Python, Ruby, Packer, Docker (Single/Multi container),
  Preconfigured Docker, custom platform.

## Deployment Modes

### Single instance

- Good for development.
- 1 EC2 instance, 1 ElasticIP, 1 ASP etc, 1 AZ.

### High Availability with Load Balancer

- Good for production.
- Multi-AZ, multiple EC2 instances, multiple ASGs, multi-AZ RDS etc.

## Deployment Policies

### All at once

- Fastest, but deploys to all instances so generates downtime.

### Rolling

- Update a few instances at a time (bucket).
- Application runs at partial capacity.
- Can control the bucket size.
- Both versions of the application are running at the same time for a period of time.
- Long deployment times.

### Rolling with additional batches

- Similar to rolling, but spins up new instances to move the batch (spin up X new instances, wait
  till healthy, discard X old instances, move on to the next batch).
- Application always runs at full capacity.
- Can control the bucket size.
- Both versions of the application are running at the same time for a period of time.
- There is additional cost due to the extra batch, that gets discarded at the end of the update.
- Good for production.

### Immutable

- Spins up new instances in a new (temporary) ASG, and once they're healthy, moves the instances 
  into the current ASG. Once health checks pass, the old instances are released.
- High cost because you're running at double capacity for a while.
- Has the longest deployment time.
- Enables fast rollback by terminating the new ASG.
- Good for production.
- No downtime.

### Traffic Splitting

- Perform canary testing as part of the deployment.
- Spins up new instances similar to an Immutable Deployment, then sends a percentage of traffic to 
  the new instances for a specific amount of time. If health checks pass, all traffic is migrated. If health check fails, canaries are terminated, and traffic is sent back to the original instances.

### Blue/Green

- Not a feature of Beanstalk.
- Zero downtime
- Create a new environment, and deploy the new version there.
- Can use Route53 to setup weighted policies to move some load over.
- Using Beanstalk, you'd swap the URLs once environment testing is finished (does a CNAME update
  via Route53)
- Is a very manual process

## Elastic Beanstalk CLI

- Additional CLI that makes working with elastic beanstalk easier.
- Common commands
  - eb create
  - eb status
  - eb health
  - eb events
  - eb logs
  - eb open
  - eb deploy
  - eb config
  - eb terminate

## Deployment Process

- Define dependencies (requirements.txt, package.json etc)
- Package code as a zip file
- Upload zip to Elastic Beanstalk to create the app version.
- Deploy the new app version to an environment.
- Elastic Beanstalk will deploy the zip to the EC2 instances, resolve dependencies & start the
  application.

## Elastic Beanstalk Lifecycle Policy

- Maximum of 1,000 application versions.
- Removes old versions of the application.
- Can be based on time (days), or space (number of versions).
- Versions in active use won't be deleted.
- There's an option to not delete the source bundle in S3 to prevent data loss.
- Need to specify the IAM role that allows Elastic Beanstalk to remove the old application version
  & associated files.

## Elastic Beanstalk Extensions

- .ebextensions folder that defines additional configurations for Elastic Beanstalk.
- Must in yaml/json format.
- File exention ends in .config (eg: logging.config)
- Can set default settings using ```option_settings```
- Add additional resources like RDS, ElastiCache etc.
- Anything managed by .ebextensions will be discarded when the environment is terminated.

## CloudFormation

- Elastic Beanstalk relies on CloudFormation.
- Can define CloudFormation resources in .ebextensions to configure basically anything.

## Cloning

- Existing environment can be cloned into a new environment.
- All resources and config are preserved (RDS data isn't).


## Beanstalk Migrations

### Loadbalancer

- Create a new environment, with the same configure, but without the Load Balancer (can't use
  clone).
- Deploy the application into the new environment.
- Do a CNAME swap/Route53 to swap the environments.

### Decoupling RDS

- Create a RDS snapshot.
- Protect RDS database from deletion in the RDS console.
- Create a new environment without RDSs, and point the application to the existing RDS.
- Do a CNAME swap/Route53 update to move traffic over.
- Terminate the old environment.
- Termination protection will prevent the RDS from being removed.
- Delete the CloudFormation stack because it will be stuck in DELETE_FAILED state.

## Single Docker

- Provide a Dockerfile to build and run the docker container, or
- A Dockerrun.aws.json that describes where the already built docker image is
- Doesn't use ECS.

## Multi-Container Docker

- Uses ECS.
- Provide a Dockerrun.aws.json at the root of the source code.
- Generates the ECS task definition.
- Docker images must already be built.


## HTTPS

### Beanstalk with HTTPS
- Load the SSL cert onto the load balancer
- Do it from the EB console.
- Do it in code using ```.ebextensions/securelistener-alb.confg```.
- SSL cert can be provisioned via ACM or CLI.
- Need to configure the SG to allow inbound HTTPS traffic.

### Beanstalk with HTTPS redirect

- Configure instances to do HTTP => HTTPS redirect.
- Configure ALB to do the redirect.
- Make sure health checks aren't redirected.

## Custom Platform

- Define from scratch OS, additional software and scripts that run.
- Only use case is when the application language doesn't use docker or is incompatible with EB.
- Use platform.yaml to define the AMI.
- Use packer to generate the AMI.

## Monitoring

- Monitor CPU/Network utilization, response time, total request count via the Elastic Beanstalk console.
- ELB and EC2 instance metrics are enabled for all environments (sent every 5mins).
- Enabled ```Enhanced Health``` to get additional metrics, such as severity of the health problem. Also enables the ```Health``` page. Metrics are sent every 10secs.
- Health agent runs on each EC2 instance to monitor web server logs and system metrics.
