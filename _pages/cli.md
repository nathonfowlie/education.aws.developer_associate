---
title: "Command Line Interface (CLI)"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /cli/
sidebar:
  nav: "docs"
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
---

## Overview

There's several ways to develop on AWS -

- AWS CLI from local machine or EC2 instance.
- AWS SDK from local machine or EC2 instance.
- AWS Instance Metadata Server for EC2.

## Configuration

### Local Environment

1. Open up IAM console and select the ```Security Credentials``` tab.
2. Click ```Create access key```.
3. Save the details somewhere secure.
4. Run ```aws configure``` in a command prompt.
5. Provide the access key and secret access key when prompted.
6. Set the default region (default: ```us-east-1```).
7. Select the default output format (default: ```json```).
8. Config files are written to ```$HOME/.aws/config``` and ```$HOME/.aws/credentials```.

### EC2 Instance

- Use an IAM role to allow the EC2 instance to make certain API calls, don't store credentials on the instance.
- EC2 instances will use the profiles/roles automatically.

## Credentials Provider Chain

The CLI looks for credentials in the following order:

1. Command line options (```--region```, ```--output```, and ```--profile```).
2. Environment variables (```AWS_ACCESS_KEY_ID```, ```AWS_SECRET_ACCESS_KEY```, ```AWS_SESSION_TOKEN```).
3. Credentials file (```$HOME/.aws/credentials```).
4. Configuration file (```$HOME/.aws/config```).
5. Container credentials (for ECS tasks).
6. Instance profile credentials (for EC2 instance profiles).

## Managing Profiles

Additional profiles can be created to make it easy to switch between different accounts.
Each profile is stored as a ini section in ```$HOME/.aws/config``` and ```$HOME/.aws/credentials```.

    $ aws configure --profile some_name
    AWS Access Key ID [None]: <some value>
    AWS Secret Access Key [None]: <some value>
    Default region name [None]: <some value>
    Default output format [None]: <some value>
    
    $ aws s3 ls --profile some_profile some_command
    ...

## Multi-factor Authentication (MFA)

1. Use IAM to assign a MFA device to the user.
2. Create a temporary session

        aws sts get-session-token --serial-number arn-of-the-mfa-device --token-code code-from-token --duration-seconds 3600

3. Run ```aws configure``` again and provide the temporary credentials.

        aws configure --profile some_profile
        AWS Access Key ID [None]: <value from returned json>
        AWS Secret Access Key [None]: <value from returned json>
        Default region name [None]: <whatever>
        Default output format [None]: <whatever>

4. Edit ```$HOME/.aws/credentials``` and add the session token:

        [some_profile]
        aws_acess_key_id = <value from returned json>
        aws_secret_access_key = <value from returned json>
        aws_session_token = <value from returned json>

## Troubleshooting

- If you get the error ```aws: command found```, Check that the ```aws``` executable is on the PATH.
- Authorization errors can be decoded  using STS

        aws sts decode-authorization-message --encoded-message <value>
