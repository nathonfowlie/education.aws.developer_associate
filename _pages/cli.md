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

## CLI Troubleshooting

- If you get the error ```aws: command found```, Check that the ```aws``` executable is on the PATH.

## CLI Configuration

1. Open up IAM console and select the ```Security Credentials``` tab.
2. Click ```Create access key```.
3. Save the details somewhere secure.
4. Run ```aws configure``` in a command prompt.
5. Provide the access key and secret access key when prompted.
6. Set the default region (eg: ap-southwest-1a)
7. Select the default output format (default is json if not specified).
8. Config files are written to ```$HOME/.aws/config``` and ```$HOME/.aws/credentials```.

### AWS CLI Configuration on EC2

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
