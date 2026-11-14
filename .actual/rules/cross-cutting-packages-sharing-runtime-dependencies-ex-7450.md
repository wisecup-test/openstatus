---
version: 2
name: cross-cutting-packages-sharing-runtime-dependencies-ex-7450
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Packages Sharing Runtime Dependencies Export Typed"
metadata:
  actual:
    adr_id: 74503b09-e71f-4e02-b1a3-9155b417f45d
    status: accepted
    candidate_group_id: 879cd289-00f5-4876-a548-936bf328492c
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Packages Sharing Runtime Dependencies Export Typed

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: Packages sharing runtime dependencies SHOULD export typed environment objects to provide compile-time guarantees across module boundaries.

### Verify

```bash
# Discover and run the project static type checker to verify environment schema definitions
npm run typecheck

# Discover and execute the package test suite to verify that invalid environment configurations throw validation errors at startup
npm test
```

**Accept when:**
- Static type analysis passes with zero diagnostics on environment schema definitions.
- Automated test suites confirm that missing required environment variables cause immediate initialization failure with schema diagnostic messages.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>