---
version: 2
name: cross-cutting-before-consuming-updating-external-depen-8c69
description: "Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Before Consuming Updating External Dependencies Related"
metadata:
  actual:
    adr_id: 8c69918c-d0a9-4887-9d7e-f0f3d2d937f5
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

# Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Before Consuming Updating External Dependencies Related

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SSO-001** MUST: Before consuming or updating external dependencies related to single sign-on services, consumers MUST inspect the repository lock artifact and verify the exact resolved versions.

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