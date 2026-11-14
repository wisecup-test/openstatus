---
version: 2
name: cross-cutting-environment-variable-schemas-defined-oss-e047
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Environment Variable Schemas Defined Oss Env"
metadata:
  actual:
    adr_id: e047652b-acd3-4142-a284-db61e22a5420
    status: accepted
    candidate_group_id: 879cd289-00f5-4876-a548-936bf328492c
    evidence:
      source: deep
  scope:
    root: packages/notifications/bird-whatsapp
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Environment Variable Schemas Defined Oss Env

Scope: applies to `packages/notifications/bird-whatsapp/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Environment variable schemas defined with @t3-oss/env-core MUST declare runtime validators using zod schema validators including z.string().

### Verify

```bash
# Discover and run the project static type checker to verify that environment schema definitions compile without diagnostic errors.
# Discover and execute the package test suite to verify that invalid environment configurations throw validation errors at startup.
```

**Accept when:**
- Static type analysis passes with zero diagnostics on environment schema definitions.
- Automated test suites confirm that missing required environment variables cause immediate initialization failure with schema diagnostic messages.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>