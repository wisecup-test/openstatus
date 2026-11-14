---
version: 2
name: cross-cutting-application-layer-controllers-components-1199
description: "Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Application Layer Controllers Components Not Establish"
metadata:
  actual:
    adr_id: 1199c49c-bbbe-4bbe-a872-0f6a529f4327
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

# Adoption of @openstatus/services/sso for Enterprise Single Sign-On Services: Application Layer Controllers Components Not Establish

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SSO-001** SHOULD_NOT: Application-layer controllers and UI components SHOULD NOT establish direct dependencies on external identity provider client libraries or duplicate workspace lookup routines.

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