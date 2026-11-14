---
version: 2
name: cross-cutting-every-schema-table-representing-tenant-2ee1
description: "Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Every Schema Table Representing Tenant Scoped"
metadata:
  actual:
    adr_id: 2ee1ddb7-e846-41a3-afd7-001cfe63bc92
    status: accepted
    candidate_group_id: fdb61eb2-7863-4dfc-bdc0-d33078772e63
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Every Schema Table Representing Tenant Scoped

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all persistent database table definitions in the data tier representing multi-tenant resources, event telemetry, external service components, and status reports.

### Rules

- **R-DRIZZLE-001** MUST: Every schema table representing a tenant-scoped resource MUST declare an explicit index covering the tenant identifier column using the index builder.

### Verify

```bash
# Discover the project test runner from the root manifest and execute schema validation test suites to verify index coverage.
# Locate the repository schema migration verification script and execute it to confirm schema definitions align with the database catalog.
```

**Accept when:**
- All database schema definitions compile without type errors and declare required index definitions on tenant partition columns.
- Schema migration checks confirm that all declarative unique constraints and compound indices match the database catalog.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated schema verification, static type checking, and mandatory peer review enforce these rules.
</enforcement>