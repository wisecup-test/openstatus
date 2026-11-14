---
version: 2
name: cross-cutting-composite-business-keys-requiring-unique-32b3
description: "Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Composite Business Keys Requiring Uniqueness Across"
metadata:
  actual:
    adr_id: 32b371cf-0a05-464a-9216-026f75e54cd4
    status: accepted
    candidate_group_id: fdb61eb2-7863-4dfc-bdc0-d33078772e63
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Drizzle ORM Declarative Schema Indexing and Tenant Boundary Isolation: Composite Business Keys Requiring Uniqueness Across

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Composite business keys requiring uniqueness across external entity integrations or temporal periods MUST be declared using the uniqueIndex or unique builders within the table configuration.

### Verify

```bash
# Discover the project test runner from the root manifest and execute schema validation test suites to verify index coverage.
# Locate the repository schema migration verification script and execute it to confirm schema definitions align with the database catalog.
```

**Accept when:**
- All database schema definitions compile without type errors and declare required index definitions on tenant partition columns.
- Schema migration checks confirm that all declarative unique constraints and compound indices match the database catalog.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>