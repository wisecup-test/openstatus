---
version: 2
name: cross-cutting-relational-database-queries-constructed-66a3
description: "Drizzle ORM and LibSQL Database Access Layer: Relational Database Queries Constructed Through Drizzle"
metadata:
  actual:
    adr_id: 66a32ffa-9a3a-4f56-87cd-d70609371777
    status: accepted
    candidate_group_id: 294806cd-5041-4fcf-9a73-6e2dea7de009
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# Drizzle ORM and LibSQL Database Access Layer: Relational Database Queries Constructed Through Drizzle

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Relational database queries MUST be constructed through Drizzle ORM schema-driven abstractions rather than untyped raw string execution.

### Verify

```bash
# Discover and execute the repository type-checking script to validate database types
# Discover and run the project's test suite to verify database connection initialization and query operations
```

**Accept when:**
- Static type analysis completes with zero diagnostic errors across all database schema, query, and migration modules.
- Database test executions succeed against configured connection parameters without connection or query failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>