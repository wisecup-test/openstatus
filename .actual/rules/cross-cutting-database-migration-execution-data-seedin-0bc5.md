---
version: 2
name: cross-cutting-database-migration-execution-data-seedin-0bc5
description: "@libsql/client Adoption: Database Migration Execution Data Seeding Routines"
metadata:
  actual:
    adr_id: 0bc5c923-8e84-4bfc-b695-18622bdb0028
    status: accepted
    candidate_group_id: e0108034-3797-4419-a73d-ebc4c6b1fec3
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# @libsql/client Adoption: Database Migration Execution Data Seeding Routines

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all database modules responsible for migrations, data seeding, and runtime query execution, as well as embedded replica synchronization routines.

### Rules

- **R-LIB-001** MUST: Database migration execution and data seeding routines MUST execute using client instances generated through @libsql/client.

### Verify

```bash
# Discover and run the project type-checking script to verify @libsql/client interface compatibility.
# Discover and run the automated test suite to validate client connectivity and migration execution.
# Discover and run the linting suite to ensure database client imports conform to project boundary constraints.
```

**Accept when:**
- Static type checks verify interface compatibility for all client initializations without diagnostics.
- All database connectivity, migration, and replication test suites pass successfully.
- Repository dependency manifests contain no unapproved alternative relational database drivers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, static type checks, and architectural code reviews enforce compliance.
</enforcement>