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
- Never store AWS credentials in code (duh). Use the credentials chain
- Use IAM roles as much as possible within AWS.
- Use environment variables or a named profile when working outside of AWS.

## IAM Federation

Integrates corporate user respositories via SAML (LDAP etc).

## IAM Roles

- Common roles are EC2 instance roles, Lambda Function roles and CloudFormation roles.

## IAM Policies

- AWS Managed policies are managed by AWS. Good for power users & administrators. They get updated when there's changes to AWS services, or new services made available.
- Customer Managed policies are re-usable against many principals. They're version controlled, support rollback, and have central change management.
- Inline policies apply to a single principal. No version control, no rollback, deleted if the IAM principal is deleted, and limited to 2KB.
- IAM policies apply to users, roles and groups.
- S3 policies apply to specific buckets.
- S3 bucket permissions are based on the union of the IAM policy(s) and S3 policy(s).

### Policy Types

| Type                                      | Description                                                                                       |
|-------------------------------------------|---------------------------------------------------------------------------------------------------|
| Identity-based                            | Managed/inline policies that allow/deny permissions for a specific identity (user, group, role).  |
| Resource-based                            | Inline policies attached to resources to allow/deny access.                                       |
| Permissions Boundaries                    | Defines the maximum permissions for an identity. Doesn't grant permissions.                       |
| Organisation Service Control Policy (SCP) | Defines maximum permissions for account members of an organisation or OU. Only denys permissions. |
| Access Control List (ACL)                 | Grants permissions on resources in other accounts (cross-account).                                |
| Session                                   | Limits permissions when assuming a role or federated user. .                                      |

### Policy Evaluation Order

1. If there's an explicit DENY, deny access.
2. If there's an explicit ALLOW, allow access.
3. Otherwise, deny access.

### Dynamic Policies

Policy variables can be used to insert dynamic information (ie:```${aws:username}```).

``` json
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

#### Example role passing policy

``` json
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

### Trust Policies

- Defines which IAM Principle (accounts, users or roles) is allowed to assume a role.
- Only type of resource policy supported by IAM.


#### Example trust policy

``` json
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

## IAM Security Tools

### IAM Credentials Report

- Account level.
- Lists the users in an account and the status of their credentials.

### Access Advisor

- Operates at the User level.
- Service permissions granted to a user, and when the services were last accessed.
- Check for unused roles.

### Access Analyzer

- Identify resources that are shared with an external entity (s3 buckets, iam roles etc).
- Find unintended access to resources & data.

### AWS Policy Simulator

- Online tool to test IAM policies.
- Can test the effect of policies attached to a user, group or role
    - Select the user/group/role.
    - Select the service(s) to test the policy against.
    - Select the API action to test the policy against.
    
### AWS CLI Dry Runs

Use the ```--dry-run``` arg to validate permissions without creating resources. For example:

``` shell
$ aws ec2 run-instances --dry-run --image-id ami-06340c8c12baa6a09 --instance-type t2.micro
```

## CloudShell

- Access from the management console.
- Default region is the region that you're logged into within the console.
- Files created within CloudShell are persisted.
- Can download/upload files.
- Supports multiple tabs (shells).
- Only available in some regions.
