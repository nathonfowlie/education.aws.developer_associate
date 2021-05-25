# Identity & Access Management

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

- AWS Managed policies are managed by AWS. Good for power users & administrators. They get updated when there's changes to AWS services, or new services made available.
- Customer Managed policies are re-usable against many principals. They're version controlled, support rollback, and have central change management.
- Inline policies apply to a single principal. No version control, no rollback, deleted if the IAM principal is deleted, and limited to 2KB.
- IAM policies apply to users, roles and groups.
- S3 policies apply to specific buckets.
- S3 bucket permissions are based on the union of the IAM policy(s) and S3 policy(s).

### Policy Evaluation Order

1. If there's an explicit DENY, deny access.
2. If there's an explicit ALLOW, allow access.
3. Otherwise, deny access.

### Dynamic Policies

Policy variables can be used to insert dynamic information (ie:```${aws:username}```).

```
{
    "Sid": "AllowAllS3ActionsInUserFolder",
    "Action": ["s3:*"],
    "Effect": "Allow",
    "Resource": ["arn:aws:s3:::my-company/home/${aws:username}/*"]
```

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

### Granting User Permissions to pass a role to an AWS Service

- AWS Services must be passed a role in order to configure them. They'll assume this role to perform actions.
- Requires the ```iam:PassRole``` permission.
- Usually used in combination with ```iam:GetRole``` to view the role being passed.
- Roles can only be passed to what their ```Trust``` allows.
- A ```trust policy``` defines which service is allowed to assume a role.

#### Example role passing policy

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam::123456789012:role/S3Access"
    }
  ]
}
```

#### Example trust policy

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "TrustPolicyStatementThatAllowsEC2ServiceToAssumeTheAttachedRole",
    "Effect": "Allow",
    "Principal": {
      "Service": "ec2.amazonaws.com"
    },
    "Action": "sts:AssumeRole"
  }
}
```

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
