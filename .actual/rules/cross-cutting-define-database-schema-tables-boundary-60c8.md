---
version: 2
name: cross-cutting-define-database-schema-tables-boundary-60c8
description: "drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Define Database Schema Tables Boundary Entities"
metadata:
  actual:
    adr_id: 60c8162b-ec08-40e9-8a40-b758602b3429
    status: accepted
    candidate_group_id: beb1e334-963d-4d7e-92c8-5bf018e37015
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Define Database Schema Tables Boundary Entities

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Define all database schema tables and boundary entities using drizzle-orm/sqlite-core schema constructs with explicit column definitions.

### Verify

```bash
# Discover and execute the project database schema check script to validate index definitions
# Discover and execute the project test runner to verify schema constraint and query performance tests
```

**Accept when:**
- All schema definitions declare explicit indexes for tenant and event queries
- Schema verification checks pass without missing index warnings or type errors
- Integration event lookups adhere to indexed column ordering

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>