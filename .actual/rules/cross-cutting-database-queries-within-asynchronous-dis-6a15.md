---
version: 2
name: cross-cutting-database-queries-within-asynchronous-dis-6a15
description: "Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Database Queries Within Asynchronous Dispatch Pipelines"
metadata:
  actual:
    adr_id: 6a15d30d-5cea-4b70-b2c1-2a783a8edba9
    status: accepted
    candidate_group_id: d92b2fc8-976d-4c73-9cf2-ca95584cdb7a
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/**
---

# Adoption of @openstatus/db Shared Workspace Module for Persistence and Schema Access: Database Queries Within Asynchronous Dispatch Pipelines

Scope: applies to `packages/subscriptions/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Database queries within asynchronous dispatch pipelines MUST utilize the schema query interface from @openstatus/db with explicit filtering predicates and deterministic ordering.

### Verify

```bash
# Discover and run repository static analysis, linting, type-checking, and tests to verify schema usage and module boundaries.
# (Commands must be discovered from the repository manifest and scripts)
```

**Accept when:**
- Static analysis confirms zero unauthorized database driver imports outside of the designated shared database module.
- All automated tests for notification dispatch and channel handlers pass with complete type verification and schema conformity.
- Entity queries in dispatch routines execute with deterministic sorting and explicit filtering without runtime schema errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated static analysis, module boundary linting, workspace type checking, and automated test execution is mandatory.
</enforcement>