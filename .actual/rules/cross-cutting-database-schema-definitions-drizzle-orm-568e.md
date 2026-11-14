---
version: 2
name: cross-cutting-database-schema-definitions-drizzle-orm-568e
description: "Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Database Schema Definitions Drizzle Orm Explicitly"
metadata:
  actual:
    adr_id: 568e31f9-58cf-4d85-9f0d-b3058eff0e72
    status: accepted
    candidate_group_id: fdb61eb2-7863-4dfc-bdc0-d33078772e63
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Database Schema Definitions Drizzle Orm Explicitly

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all persistent database table definitions in the data tier, including relational entities representing multi-tenant resources, event telemetry, external service components, and status reports (excluding in-memory ephemeral cache stores, stateless client-side data structures, transit payload models, and single-row configuration tables or static lookup enumerations per exception EXC-001).

### Rules

- **R-DRIZZLE-001** MUST: Database schema definitions using Drizzle ORM MUST explicitly declare index and uniqueness constraints within table configuration callbacks for all multi-tenant partition keys, external entity integrations, and timestamped query boundaries.

### Verify

```bash
# Discover the project test runner from the root manifest and execute schema validation test suites to verify index coverage.
# Locate the repository schema migration verification script and execute it to confirm schema definitions align with the database catalog.
```

**Accept when:**
- All database schema definitions compile without type errors and declare required index definitions on tenant partition columns.
- Schema migration checks confirm that all declarative unique constraints and compound indices match the database catalog.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration automated schema verification, static type checking, and mandatory peer review on all schema definitions and migration diffs.
</enforcement>