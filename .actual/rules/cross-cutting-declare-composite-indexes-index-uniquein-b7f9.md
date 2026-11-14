---
version: 2
name: cross-cutting-declare-composite-indexes-index-uniquein-b7f9
description: "drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Declare Composite Indexes Index Uniqueindex Drizzle"
metadata:
  actual:
    adr_id: b7f91e61-63dc-4acb-9dcc-7bac4fce99d5
    status: accepted
    candidate_group_id: beb1e334-963d-4d7e-92c8-5bf018e37015
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Declare Composite Indexes Index Uniqueindex Drizzle

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Declare composite indexes using index or uniqueIndex from drizzle-orm/sqlite-core covering tenant scope keys alongside event timestamps or entity identifiers for event-driven queries.

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