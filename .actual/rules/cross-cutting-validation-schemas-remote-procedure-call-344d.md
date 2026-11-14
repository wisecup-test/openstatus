---
version: 2
name: cross-cutting-validation-schemas-remote-procedure-call-344d
description: "Adoption of @openstatus/db Schema Constants Module: Validation Schemas Remote Procedure Call Converters"
metadata:
  actual:
    adr_id: 344d1162-3c75-4d16-99c4-d3722c915cdf
    status: accepted
    candidate_group_id: 0836545d-e9dc-49d4-a1be-1f6156507f7c
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @openstatus/db Schema Constants Module: Validation Schemas Remote Procedure Call Converters

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for validation schemas, remote procedure call converters, and server routing modules interacting with domain constants or database entities.

### Rules

- **R-DB-001** SHOULD: Validation schemas and remote procedure call converters directly reference exported constants from @openstatus/db/src/schema/constants when constructing schema rules and type transformations.
- **R-DB-002** MUST: When introducing new domain constants, declare them alongside corresponding schema entities within @openstatus/db and export them via the schema constants module.
- **R-DB-003** MUST: Ensure that input validation schemas and remote procedure call converters consume exported constants rather than hardcoded primitive values.

### Verify

```bash
# Discover and execute the primary package configuration script for static type verification
pnpm typecheck

# Discover the workspace test execution script and run the automated test suite
pnpm test

# Inspect the repository script definitions and execute the linter to verify import boundary constraints
pnpm lint
```

**Accept when:**
- Static type verification passes without type errors across all modules importing @openstatus/db schema constants.
- Automated unit and integration test suites pass with zero failures across consuming service handlers.
- Static analysis passes with zero import boundary or syntax violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>