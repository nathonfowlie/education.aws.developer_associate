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

