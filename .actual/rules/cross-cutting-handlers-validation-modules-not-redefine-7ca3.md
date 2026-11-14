---
version: 2
name: cross-cutting-handlers-validation-modules-not-redefine-7ca3
description: "Adoption of @openstatus/db Schema Constants Module: Handlers Validation Modules Not Redefine Database"
metadata:
  actual:
    adr_id: 7ca3a3e6-f975-44cb-b549-59372f3b28f9
    status: accepted
    candidate_group_id: 0836545d-e9dc-49d4-a1be-1f6156507f7c
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Adoption of @openstatus/db Schema Constants Module: Handlers Validation Modules Not Redefine Database

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for server routing modules, remote procedure call handlers, validation schemas, environment configuration, and converter modules interacting with domain constants or database entities.

### Rules

- **R-DB-001** MUST_NOT: Handlers and validation modules MUST NOT redefine database schema constants, enumeration values, or plan limit thresholds independently of @openstatus/db/src/schema/constants.

### Verify

```bash
# Discover and run the primary package configuration script for static type verification
pnpm typecheck

# Discover the workspace test execution script and run the automated test suite against consuming packages
pnpm test

# Inspect the repository script definitions and execute the linter to verify import boundary constraints
pnpm lint
```

**Accept when:**
- Static type verification passes without type errors across all modules importing @openstatus/db schema constants.
- Automated unit and integration test suites pass with zero failures across consuming service handlers.
- Static analysis passes with zero import boundary or syntax violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing duplicated domain constants or unapproved module imports will fail automated validation, and engineers must replace duplicated constants with centralized imports from @openstatus/db/src/schema/constants before merging.
</enforcement>