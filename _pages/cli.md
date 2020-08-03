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

- AWS CLI from local machine
- AWS CLI from EC2 instance
- AWS SDK from local machine
- AWS SDK from EC2 instance
- AWS Instance Metadata Server for EC2

## Configuration

1. Open up IAM console and select the ```Security Credentials``` tab.
2. Click ```Create access key```.
3. Save the details somewhere secure.
4. Run ```aws configure``` in a command prompt.
5. Provide the access key and secret access key when prompted.
6. Set the default region (eg: ap-southwest-1a)
7. Select the default output format (default is json if not specified).
8. Config files are written to ```$HOME/.aws/config``` and ```$HOME/.aws/credentials```.

### Configuration on EC2

- Use an IAM role, don't store credentials on the instance.
- EC2 instances will use the profiles/roles automatically.

1. Run ```aws configure```.
2. Don't specify anything for access or secret key
3. Set the default region
4. Set the default output format
5. In IAM, create a new role attached to an AWS service
6. Select (EC2)
7. Click next
8. Select the relevant services (eg: AmazonS3ReadOnlyAccess)
9. Click next
10. Set the role name and description
11. Click next and finish.
12. Attach the new IAM role to the EC2 instance.

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

## Multi-factor Authentication

1. Need to assign a MFA device in IAM to the user. Usual process:
    a. Scan QR code.
    b. Enter 2 consecutive codes.
2. Create a temporary session

        aws sts get-session-token --serial-number arn-of-the-mfa-device --token-code code-from-token --duration-seconds 3600

3. Take not of the temporary access key,  session token and access key id.
4. Run ```aws configure``` again and provide the temporary credentials.

        aws configure --profile some_profile
        AWS Access Key ID [None]: <value from returned json>
        AWS Secret Access Key [None]: <value from returned json>
        Default region name [None]: <whatever>
        Default output format [None]: <whatever>

5. Edit ```$HOME/.aws/credentials``` and add the session token:

        [some_profile]
        aws_acess_key_id = <value from returned json>
        aws_secret_access_key = <value from returned json>
        aws_session_token = <value from returned json>

6. Now API calls using that profile will use the temporary credentials.

## Troubleshooting

- If you get the error ```aws: command found```, Check that the ```aws``` executable is on the PATH.
- Authorization errors can be decoded  using STS

        aws sts decode-authorization-message --encoded-message <value>
