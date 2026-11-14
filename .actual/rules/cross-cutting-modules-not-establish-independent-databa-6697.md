---
version: 2
name: cross-cutting-modules-not-establish-independent-databa-6697
description: "Adoption of @openstatus/db for Data Access and Schema Definition: Modules Not Establish Independent Database Connections"
metadata:
  actual:
    adr_id: 66970e1c-fa88-4207-95f3-32bed9ccb0fe
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

# Adoption of @openstatus/db for Data Access and Schema Definition: Modules Not Establish Independent Database Connections

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`.

These rules are ALWAYS ACTIVE for background workflow handlers, scheduled tasks, and operational utility routines requiring relational database persistence or entity schema validation.

### Rules

- **R-DB-001** MUST_NOT: Modules MUST NOT establish independent database connections or declare duplicate relational schema definitions outside the designated shared database package.

### Verify

```bash
# Discover and execute the project typecheck script
# Discover and execute the project test suite covering background workflow jobs and database operations
```

**Accept when:**
- All workflow and task handlers compile without static type errors against the shared database package schema definitions.
- Database queries, mutations, and transactional operations pass all automated test suites without unhandled runtime exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced via automated continuous integration checks, static type inspection, test suites, and peer code review.
</enforcement>