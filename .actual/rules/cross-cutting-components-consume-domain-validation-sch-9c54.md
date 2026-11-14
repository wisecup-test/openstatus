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
- **R-DB-002** MANDATORY: The consumer MUST discover dependency manifests, build tools, lock files, and typecheck/test scripts from the project repository.

### Verify

```bash
# Discover and execute the project typecheck script
# Discover and execute the project test suite covering background workflow jobs and database operations
```

**Accept when:**
- All workflow and task handlers compile without static type errors against the shared database package schema definitions.
- Database queries, mutations, and transactional operations pass all automated test suites without unhandled runtime exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All persistent data interactions must import from the designated shared package, and verification commands must be executed to ensure compliance.
</enforcement>