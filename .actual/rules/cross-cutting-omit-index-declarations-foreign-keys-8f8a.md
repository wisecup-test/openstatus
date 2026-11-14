---
version: 2
name: cross-cutting-omit-index-declarations-foreign-keys-8f8a
description: "drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Omit Index Declarations Foreign Keys Tenant"
metadata:
  actual:
    adr_id: 8f8afbf1-34bd-42c3-bcdc-68156aa09262
    status: accepted
    candidate_group_id: beb1e334-963d-4d7e-92c8-5bf018e37015
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Omit Index Declarations Foreign Keys Tenant

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all relational database schema definitions modeling integration entities, event logs, monitor runs, audit trails, and table definitions establishing tenancy boundaries or temporal status tracking.

### Rules

- **R-DRIZZLE-001** MUST_NOT: Omit index declarations on foreign keys and tenant identifiers that participate in event filtering or join operations.

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
Claude Code MUST NOT skip or defer verification. Schema additions without required composite tenant and event indexes will fail automated review checks and pull requests introducing unindexed temporal queries will be blocked.
</enforcement>