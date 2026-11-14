---
version: 2
name: cross-cutting-application-service-handlers-route-schem-0b46
description: "Adoption of @openstatus/db Schema Constants Module: Application Service Handlers Route Schemas Converters"
metadata:
  actual:
    adr_id: 0b46103b-278f-4d77-9604-881acd774381
    status: accepted
    candidate_group_id: 0836545d-e9dc-49d4-a1be-1f6156507f7c
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @openstatus/db Schema Constants Module: Application Service Handlers Route Schemas Converters

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all application service handlers, route schemas, converters, validation routines, and environment configuration modules that interact with domain constants or database entities.

### Rules

- **R-DB-001** MUST: All application service handlers, route schemas, converters, and validation routines MUST import shared domain constants and schema definitions directly from `@openstatus/db/src/schema/constants` rather than declaring duplicate local constants.

### Verify

```bash
# Discover and execute the primary package configuration script for static type verification
pnpm typecheck

# Discover the workspace test execution script and run the automated test suite against consuming packages
pnpm test

# Inspect repository script definitions and execute the linter to verify import boundary constraints
pnpm lint
```

**Accept when:**
- Static type verification passes without type errors across all modules importing `@openstatus/db` schema constants.
- Automated unit and integration test suites pass with zero failures across consuming service handlers.
- Static analysis passes with zero import boundary or syntax violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via the repository scripts.
</enforcement>