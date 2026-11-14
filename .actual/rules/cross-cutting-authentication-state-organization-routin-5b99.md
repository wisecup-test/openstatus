---
version: 2
name: cross-cutting-authentication-state-organization-routin-5b99
description: "Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Authentication State Organization Routing Cookies Set"
metadata:
  actual:
    adr_id: 5b99c4c5-de0a-4610-adf9-0cfaaa7f89e9
    status: accepted
    candidate_group_id: d98bb90f-25e2-42e0-bf3f-669cc5c4be85
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/lib/auth/**
---

# Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Authentication State Organization Routing Cookies Set

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all server actions, route handlers, and webhook receivers implementing enterprise authentication or single sign-on, and internal service modules managing directory synchronization and identity provider interactions.

### Rules

- **R-SSO-001** MUST: All authentication state and organization routing cookies set during single sign-on flows MUST enforce secure attributes including HTTP-only flags, production transport encryption, and restricted path scopes.

### Verify

```bash
# Discover the repository test runner from the build manifest and execute test suites covering identity provider and authentication modules.
# Discover the repository linter and static analysis configuration to verify module boundary compliance across authentication routes.
```

**Accept when:**
- Static analysis confirms all single sign-on operations, webhook handlers, and authentication actions route through @openstatus/services/sso.
- Automated test suites pass without regressions across enterprise authentication and identity provider integration paths.
- All dependencies resolve to explicit entries defined within the repository lock artifact.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>