# Directory Services

## Overview


## Microsoft Active Directory (AD)

- Database of objects (user accounts, computers, printers, file shares, security groups).
- Provides centralized security management.
- Objects organised in trees.
- Group of trees is a forest.

## AWS Directory Services

### AWS Managed Microsoft AD

- Create AD in AWS.
- Manage users locally.
- Supports MFA.
- Create trust relationships with on-prem AD.
- Standard version allows up to 30,000 objects, 1GB storage.
- Enterprise version allows up to 500,000 objects, 17GB storage.
- 

### AD Connector

- Directory Gateway (proxy) to redirect to the on-prem AD.
- Users managed locally.
- Two connectors, one for up to 500 users, another for up to 5,000 users.

### Simple AD

- AD-compatible managed directory on AWS.
- No MFA.
- Can't join on-prem AD.

### Amazon Cognito User Pools

- Redirect requests to cognito.

