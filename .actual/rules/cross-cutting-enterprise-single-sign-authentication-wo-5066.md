---
version: 2
name: cross-cutting-enterprise-single-sign-authentication-wo-5066
description: "Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Enterprise Single Sign Authentication Workflows Identity"
metadata:
  actual:
    adr_id: 50668ae2-f48d-4184-836e-fc140c4e23bc
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

# Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Enterprise Single Sign Authentication Workflows Identity

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SSO-001** MUST: All enterprise single sign-on authentication workflows, identity provider routing, and identity provider webhook handling MUST encapsulate domain operations within @openstatus/services/sso.
- **R-SSO-002** MUST: Session and state cookies emitted during single sign-on handshakes MUST enforce HTTP-only, secure, and restricted cross-site policies.
- **R-SSO-003** MUST: Identity provider signature verification routines MUST be encapsulated completely within the service boundary before payload consumption.

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