---
version: 2
name: cross-cutting-direct-database-connections-not-bypass-a7da
description: "Drizzle ORM and LibSQL Database Access Layer: Direct Database Connections Not Bypass Configured"
metadata:
  actual:
    adr_id: a7daea82-7434-43e0-8afc-fd5e73dda48f
    status: accepted
    candidate_group_id: 294806cd-5041-4fcf-9a73-6e2dea7de009
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/**
---

# Drizzle ORM and LibSQL Database Access Layer: Direct Database Connections Not Bypass Configured

Scope: applies to `packages/db/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST_NOT: Direct database connections MUST NOT bypass the configured LibSQL client or Drizzle ORM database instance.

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