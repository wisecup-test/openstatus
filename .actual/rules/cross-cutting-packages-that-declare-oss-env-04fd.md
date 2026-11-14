---
version: 2
name: cross-cutting-packages-that-declare-oss-env-04fd
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Packages That Declare Oss Env Core"
metadata:
  actual:
    adr_id: 04fd1874-2b6e-45b5-a972-513deecae210
    status: accepted
    candidate_group_id: 879cd289-00f5-4876-a548-936bf328492c
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Packages That Declare Oss Env Core

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST_NOT: Packages that declare @t3-oss/env-core configurations MUST NOT bypass schema validation by reading undeclared raw environment properties directly.

### Verify

```bash
# Discover and run the project static type checker to verify environment schema definitions
# Discover and execute the package test suite to verify that invalid environment configurations throw validation errors at startup
```

**Accept when:**
- Static type analysis passes with zero diagnostics on environment schema definitions.
- Automated test suites confirm that missing required environment variables cause immediate initialization failure with schema diagnostic messages.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>