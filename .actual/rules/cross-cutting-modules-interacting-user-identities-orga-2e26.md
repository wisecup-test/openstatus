---
version: 2
name: cross-cutting-modules-interacting-user-identities-orga-2e26
description: "Internal Authentication Module (@/lib/auth) Adoption: Modules Interacting User Identities Organization Credentials"
metadata:
  actual:
    adr_id: 2e26020b-c6d9-4443-9cc0-556e5165405f
    status: accepted
    candidate_group_id: 0fa3c1d8-a1c4-4a91-8df9-e2bd52d4825b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Internal Authentication Module (@/lib/auth) Adoption: Modules Interacting User Identities Organization Credentials

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AUTH-001** SHOULD: Modules interacting with user identities or organization credentials SHOULD delegate session parsing and identity extraction directly to the shared module interface rather than re-implementing credential validation.

### Verify

```bash
# Discover the workspace static analysis script from repository configuration and execute it across all route and service boundaries.
# Locate the repository test runner from project configuration and run the test suite covering authentication utilities and dependent route handlers.
# Query repository imports to verify all session and identity operations originate from the designated internal authentication module.
```

**Accept when:**
- All static analysis and linting checks pass without violations of module boundary constraints.
- The automated test suite passes completely, verifying session resolution and authentication flows across dependent modules.
- Zero occurrences of direct third-party authentication provider imports exist outside the designated authentication library boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>