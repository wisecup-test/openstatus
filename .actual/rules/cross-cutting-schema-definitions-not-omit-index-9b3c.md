---
version: 2
name: cross-cutting-schema-definitions-not-omit-index-9b3c
description: "Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Schema Definitions Not Omit Index Coverage"
metadata:
  actual:
    adr_id: 9b3ce69e-b161-45c7-947f-5ab5b6c09e2f
    status: accepted
    candidate_group_id: fdb61eb2-7863-4dfc-bdc0-d33078772e63
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Schema Definitions Not Omit Index Coverage

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all persistent database table definitions in the data tier, including relational entities representing multi-tenant resources, event telemetry, external service components, and status reports.

### Rules

- **R-SCHEMA-001** MUST_NOT: Schema definitions MUST NOT omit index coverage on foreign keys or cross-service boundary lookup identifiers.

### Verify

```bash
# Discover the project test runner from the root manifest and execute schema validation test suites to verify index coverage.
# Locate the repository schema migration verification script and execute it to confirm schema definitions align with the database catalog.
```

**Accept when:**
- All database schema definitions compile without type errors and declare required index definitions on tenant partition columns.
- Schema migration checks confirm that all declarative unique constraints and compound indices match the database catalog.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated schema verification and static type checking enforce these rules.
</enforcement>