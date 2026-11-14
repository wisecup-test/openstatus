---
version: 2
name: cross-cutting-embedded-replica-database-clients-coordi-cd51
description: "Drizzle ORM and LibSQL Database Access Layer: Embedded Replica Database Clients Coordinating Remote"
metadata:
  actual:
    adr_id: cd51e129-d63e-4c8f-8643-40273ded289e
    status: accepted
    candidate_group_id: 294806cd-5041-4fcf-9a73-6e2dea7de009
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# Drizzle ORM and LibSQL Database Access Layer: Embedded Replica Database Clients Coordinating Remote

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Embedded replica database clients coordinating remote synchronization MUST configure explicit local storage paths, synchronization targets, authentication tokens, and sync intervals through centralized environment configurations.

### Verify

```bash
# Discover and execute the repository type-checking script to validate database queries, migrations, and schema definitions
# Discover and run the project's test suite to verify database connection initialization, schema migration execution, and query operations
```

**Accept when:**
- Static type analysis completes with zero diagnostic errors across all database schema, query, and migration modules.
- Database test executions succeed against configured connection parameters without connection or query failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>