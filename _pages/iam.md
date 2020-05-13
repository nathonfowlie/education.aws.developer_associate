---
permalink: /iam/
toc: true
toc_label: "Table of Contents"
toc_icon: book_open
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

### Best Practise

- Delete the root access keys.
- Activate MFA.
- Create an IAM user.
- Create a group to allocate permissions.

### IAM Federation

Integrates corporate user respositories via SAML (LDAP etc).

### Roles

- One role per application.
