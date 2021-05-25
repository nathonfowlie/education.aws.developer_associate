# STS

## Overview

- Get temporary credentials to permit access to resources for up to 1hr.
- AssumeRole to assume a role within the account or cross account.
- AssumeRoleWithSAML to return credentials for users logged in using SAML tokens.
- AssumeRoleWithIdentity to return credentials for users logged in with an Identity Provider (Facebook, Google, OIDC compatible). Try to use Cognito Identity Pools instead.
- GetSessionToken for MFA. From a user, or an AWS account root user.
- GetFederatedToken for temporary credentials for a federated user.
- GetCallerIdentity to get details about an IAM user or role used in the API call.
- DecodeAuthorizationMessage to decode an error message when an AWS API is denied.

## Assuming roles using STS

- Define an IAM role.
- Define the principals that can access the role.
- Use the ```AssumeRole``` API to get credentials and impersonate the IAM role. Credentials will be valid from 15mins to an hour.

## MFA using STS

- Use GetSessionToken to authenticate using an MFA code.
- Create an IAM policy with the appropriate IAM conditions.
- Add  ```aws:MultiFactorAuthPresent:true``` to the IAM policy.
- GetSessionToken returns -
  - Access ID
  - Secret Key
  - Secret Token
  - Expiration Date
