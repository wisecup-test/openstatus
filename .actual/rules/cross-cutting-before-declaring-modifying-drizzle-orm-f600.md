---
version: 2
name: cross-cutting-before-declaring-modifying-drizzle-orm-f600
description: "Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Before Declaring Modifying Drizzle Orm Schema"
metadata:
  actual:
    adr_id: f600a6f5-32d8-45b6-babf-db15c4496222
    status: accepted
    candidate_group_id: fdb61eb2-7863-4dfc-bdc0-d33078772e63
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Before Declaring Modifying Drizzle Orm Schema

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all persistent database table definitions in the data tier and relational entities representing multi-tenant resources, event telemetry, external service components, and status reports.

### Rules

- **R-DRIZZLE-001** MUST: Before declaring or modifying Drizzle ORM schema index structures, developers MUST inspect the repository lock artifact to determine the exact resolved library version and confirm that all index builder APIs match the official documentation for that resolved version.

### Verify

```bash
# Discover the project test runner from the root manifest and execute schema validation test suites
# Locate the repository schema migration verification script and execute it to confirm schema definitions align with the database catalog
```

**Accept when:**
- All database schema definitions compile without type errors and declare required index definitions on tenant partition columns.
- Schema migration checks confirm that all declarative unique constraints and compound indices match the database catalog.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>