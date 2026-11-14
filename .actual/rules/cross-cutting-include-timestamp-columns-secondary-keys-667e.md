---
version: 2
name: cross-cutting-include-timestamp-columns-secondary-keys-667e
description: "drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Include Timestamp Columns Secondary Keys Composite"
metadata:
  actual:
    adr_id: 667e58e2-b1d7-4094-aeea-0dfa3fa3573f
    status: accepted
    candidate_group_id: beb1e334-963d-4d7e-92c8-5bf018e37015
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Include Timestamp Columns Secondary Keys Composite

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all relational database schema definitions modeling integration entities, event logs, monitor runs, and audit trails.

### Rules

- **R-REC-001** SHOULD: Include timestamp columns as secondary keys in composite indexes when queries filter or sort chronologically over integration events.

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