---
title: "Identity & Access Management"
header:
  image: "/assets/images/aws_developer_associate_teaser.jpg"
permalink: /iam/
toc: true
toc_icon: "book-reader"
toc_label: "Table of Contents"
sidebar:
  nav: "docs"
---

## Identity & Access Management (IAM)

- Consists of users (a person), groups (function/team) and roles (internal aws usage, assigned to machines).
- Policies defined in JSON.
- Global service, not bound to a region/AZ.
- Supports MFA.
- Has pre-defined policies.
- Should follow least privilege principle.

## Best Practise

- Delete the root access keys.
- Activate MFA.
- Create an IAM user.
- Create a group to allocate permissions.

## IAM Federation

Integrates corporate user respositories via SAML (LDAP etc).

## Roles

- One role per application.

## IAM Policies

- Inline policies are specific to a single role.
- Policies can be versioned.

### Creating a Policy

Three ways to create a policy via the UI:

- The visual editor.
- The AWS policy generator.
- Hand-crafted json.

Via the visual editor -

1. Select the service.
2. Select the allowed actions (list, read, write, GetBucket etc).
3. Select resources that the policy applies to (arn).
4. Specific request conditions.
5. Add additional permissions if needed.
6. Give the policy a name and description.
7. Click ```Create Policy```.
8. Select a role.
9. Attach the policy to the role.

### AWS Policy Simulator

- Online tool to test IAM policies.
- Can test the effect of policies attached to a user, group or role
    - Select the user/group/role.
    - Select the service(s) to test the policy against.
    - Select the API action to test the policy against.
    
### AWS CLI Dry Runs

Use the ```--dry-run``` arg to validate permissions without creating resources. For example:

    aws ec2 run-instances --dry-run --image-id ami-06340c8c12baa6a09 --instance-type t2.micro

## Best Practise

- Never store AWS credentials in code (duh). Use the credentials chain
- Use IAM roles as much as possible within AWS.
- Use environment variables or a named profile when working outside of AWS.
