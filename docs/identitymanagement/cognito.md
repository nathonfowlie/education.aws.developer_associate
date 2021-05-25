# Cognito

## Overview

- Gives users an identity so they can interact with the application.

## Cognito User Pools (CUP)

- Sign-in functionality for application users.
- Integration with API gateway & ALB (with listeners and rules).
- Serverless database that holds user accounts for web & mobile apps.
- Uses a username (or email address)/password combination.
- Supports password reset, email/phone number verification, MFA.
- Support federated identities (Facebook, Google, Apple, Amazon, SAML, OpenID Connect).
- User accounts can be blocked if their credentials are comprised somewhere.
- Login returns a JWT token.

### Lambda Triggers

- Allows Cognito User Pool to invoke a lambda function synchronously when certain events occur.

| User  Pool Flow | Operation            | Description                                                     |
|-----------------|----------------------|-----------------------------------------------------------------|
| Authentication  | Pre Authentication   | Custom validation to accept/deny the sign-in request.           |
| Authentication  | Post Authentication  | Event logging for custom analytics.                             |
| Authentication  | Pre Token Generation | Augment or suppress token claims.                               |
| Sign-Up         | Pre Sign-up          | Customer validation to accept/deny sign-up requests.            |
| Sign-Up         | Post Confirmation    | Custom welcome messages, or event logging for custom analytics. |
| Sign-Up         | Migrate User         | Migrate a user from an existing user directory to user pools.   |
| Messages        | Custom Message       | Advanced customisation and localisation of messages.            |
| Token Creation  | Pre Token Generation | Add/remove attributes in Id tokens.                             |

### Hosted Authentication UI

- UI to handle sign up/sign in workflows.
- Integration with OIDC, SAML, social logins etc.
- Can be customised with own logo & CSS.

### Cognito Identity Pools (Federated Identity)

- Allow external (application users) to retrieve AWS credentials so they can directly access AWS resources.
- Integrate with Cognito User Pools as an identity provider.

### Cognito Sync

- Synchronise data from mobile devices to Cognito.
- Deprecated and replaced by AppSync.

### Cognito vs IAM

- "Hundreds of users", "mobile users", "authenticate with SAML" = Cognito.

