# AWS Developer Associate 20202: Study Notes

[< Index](index.md)

## Table of Contents

1. [Identity & Access Management (IAM)](#identity--access-management-iam)
2. [Best Practise](#best-practise)
3. [IAM Federation](#iam-federation)
4. [Roles](#roles)

## Identity & Access Management (IAM)

- Consists of users (a person), groups (function/team) and roles (internal aws usage, assigned to machines).
- Policies defined in JSON.
- Global service, not bound to a region/AZ.
- Supports MFA.
- Has pre-defined policies.
- Should follow least privilege principle.

### Best Practise

- Delete the root access keys.
- Activate MFA.
- Create an IAM user.
- Create a group to allocate permissions.

### IAM Federation

Integrates corporate user respositories via SAML (LDAP etc).

### Roles

- One role per application.
