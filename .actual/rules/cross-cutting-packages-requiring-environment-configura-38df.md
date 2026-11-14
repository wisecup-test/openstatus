---
version: 2
name: cross-cutting-packages-requiring-environment-configura-38df
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Packages Requiring Environment Configuration Use Oss"
metadata:
  actual:
    adr_id: 38df5e01-4563-466b-985b-08640285f163
    status: accepted
    candidate_group_id: 879cd289-00f5-4876-a548-936bf328492c
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Packages Requiring Environment Configuration Use Oss

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for workspace packages and modules requiring runtime environment variable configuration.

### Rules

- **R-ENV-001** MUST: Packages requiring environment configuration MUST use @t3-oss/env-core to validate and access environment variable schemas.

### Verify

```bash
# Discover and run the project static type checker to verify environment schema definitions
# Discover and execute the package test suite to verify invalid environment configurations throw validation errors
```

**Accept when:**
- Static type analysis passes with zero diagnostics on environment schema definitions.
- Automated test suites confirm that missing required environment variables cause immediate initialization failure with schema diagnostic messages.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>