---
version: 2
name: cross-cutting-database-persistence-schema-definitions-2f68
description: "Drizzle ORM and LibSQL Database Access Layer: Database Persistence Schema Definitions Migrations Query"
metadata:
  actual:
    adr_id: 2f68143d-3f27-4665-971d-2704a03a92be
    status: accepted
    candidate_group_id: 294806cd-5041-4fcf-9a73-6e2dea7de009
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# Drizzle ORM and LibSQL Database Access Layer: Database Persistence Schema Definitions Migrations Query

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: All database persistence, schema definitions, migrations, and query operations MUST use Drizzle ORM with the LibSQL driver adapter.

### Verify

```bash
# Discover and execute the repository type-checking script
# Discover and run the project's test suite to verify database connection initialization, schema migration execution, and query operations
```

**Accept when:**
- Static type analysis completes with zero diagnostic errors across all database schema, query, and migration modules.
- Database test executions succeed against configured connection parameters without connection or query failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>