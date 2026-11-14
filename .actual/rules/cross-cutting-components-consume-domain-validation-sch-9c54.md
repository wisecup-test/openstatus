---
version: 2
name: cross-cutting-components-consume-domain-validation-sch-9c54
description: "Adoption of @openstatus/db for Data Access and Schema Definition: Components Consume Domain Validation Schemas Exported"
metadata:
  actual:
    adr_id: 9c5424f3-f26c-4c21-87b8-8f2d51f7b66a
    status: accepted
    candidate_group_id: da7123d9-0b29-4b39-b7d6-dfa708206c4f
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/checker/**
      - src/cron/**
      - src/incident/**
      - src/lib/**
      - src/scripts/**
---

# Adoption of @openstatus/db for Data Access and Schema Definition: Components Consume Domain Validation Schemas Exported

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`.

These rules are ALWAYS ACTIVE for all background workflow handlers, scheduled tasks, and operational utility routines requiring relational database persistence or entity schema validation.

### Rules

- **R-DB-001** SHOULD: Components SHOULD consume domain validation schemas exported by @openstatus/db to validate entity structures prior to persistence operations.

### Verify

```bash
# Discover and run typecheck script from the project manifest
# Discover and run test runner from the project manifest covering workflow jobs and database operations
```

**Accept when:**
- All workflow and task handlers compile without static type errors against the shared database package schema definitions.
- Database queries, mutations, and transactional operations pass all automated test suites without unhandled runtime exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All persistent data interactions must import from the designated shared package, and pull requests introducing direct database drivers or duplicate schema declarations outside the shared package will be blocked.
</enforcement>