---
version: 2
name: cross-cutting-schema-migrations-executed-drizzle-orm-1682
description: "Drizzle ORM and LibSQL Database Access Layer: Schema Migrations Executed Drizzle Orm Libsql"
metadata:
  actual:
    adr_id: 1682ffb9-210b-4f7b-8bad-425008d835ae
    status: accepted
    candidate_group_id: 294806cd-5041-4fcf-9a73-6e2dea7de009
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# Drizzle ORM and LibSQL Database Access Layer: Schema Migrations Executed Drizzle Orm Libsql

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Schema migrations MUST be executed using the Drizzle ORM LibSQL migrator module against centralized schema declarations.

### Verify

```bash
# Discover and execute the repository type-checking script to validate Drizzle ORM and LibSQL types
npm run typecheck
# Discover and run the project's test suite to verify database connection and migration execution
npm test
```

**Accept when:**
- Static type analysis completes with zero diagnostic errors across all database schema, query, and migration modules.
- Database test executions succeed against configured connection parameters without connection or query failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database queries, schema definitions, and migrations must be verified via type checking and test suites.
</enforcement>