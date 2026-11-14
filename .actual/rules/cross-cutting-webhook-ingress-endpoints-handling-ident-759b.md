---
version: 2
name: cross-cutting-webhook-ingress-endpoints-handling-ident-759b
description: "Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Webhook Ingress Endpoints Handling Identity Provider"
metadata:
  actual:
    adr_id: 759bdbea-a2b1-462f-8366-99d19d995c6a
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

# Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Webhook Ingress Endpoints Handling Identity Provider

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SSO-001** MUST: Webhook ingress endpoints handling identity provider notifications MUST validate signatures through @openstatus/services/sso before processing state changes or persisting records.

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