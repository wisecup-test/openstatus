---
version: 2
name: cross-cutting-persistent-data-access-query-execution-ef7d
description: "Adoption of @openstatus/db for Data Access and Schema Definition: Persistent Data Access Query Execution Schema"
metadata:
  actual:
    adr_id: ef7d3651-fc9e-4deb-b4d3-7f18ab60fda1
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

# Adoption of @openstatus/db for Data Access and Schema Definition: Persistent Data Access Query Execution Schema

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`.

These rules are ALWAYS ACTIVE for background workflow handlers, scheduled tasks, and operational utility routines requiring relational database persistence or entity schema validation.

### Rules

- **R-DB-001** MUST: All persistent data access, query execution, and schema references across background workflows and task runners MUST route through the @openstatus/db shared package.

### Verify

```bash
# Discover and execute the repository typecheck script to ensure database queries and schema references satisfy static types.
# Discover and execute the repository test runner covering background workflow jobs and database operations.
```

**Accept when:**
- All workflow and task handlers compile without static type errors against the shared database package schema definitions.
- Database queries, mutations, and transactional operations pass all automated test suites without unhandled runtime exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>