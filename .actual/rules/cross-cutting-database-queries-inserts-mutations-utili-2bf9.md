---
version: 2
name: cross-cutting-database-queries-inserts-mutations-utili-2bf9
description: "Adoption of @openstatus/db for Data Access and Schema Definition: Database Queries Inserts Mutations Utilize Query"
metadata:
  actual:
    adr_id: 2bf92c62-bdaa-46cc-9042-880cf33016f9
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

# Adoption of @openstatus/db for Data Access and Schema Definition: Database Queries Inserts Mutations Utilize Query

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`.

These rules are ALWAYS ACTIVE for background workflow handlers, scheduled tasks, and operational utility routines requiring relational database persistence or entity schema validation.

### Rules

- **R-DB-001** MUST: Database queries, inserts, and mutations MUST utilize the query builder methods and schema objects provided directly by @openstatus/db.

### Verify

```bash
# Discover and run the project typecheck script
# Example: pnpm typecheck

# Discover and run the project test suite
# Example: pnpm test
```

**Accept when:**
- All workflow and task handlers compile without static type errors against the shared database package schema definitions.
- Database queries, mutations, and transactional operations pass all automated test suites without unhandled runtime exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All persistent data interactions must import from the designated shared package, verified by automated continuous integration checks and peer code review.
</enforcement>