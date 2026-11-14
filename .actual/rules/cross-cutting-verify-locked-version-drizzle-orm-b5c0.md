---
version: 2
name: cross-cutting-verify-locked-version-drizzle-orm-b5c0
description: "drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Verify Locked Version Drizzle Orm Repository"
metadata:
  actual:
    adr_id: b5c00cc4-6508-47f7-b029-e959df807e42
    status: accepted
    candidate_group_id: beb1e334-963d-4d7e-92c8-5bf018e37015
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# drizzle-orm/sqlite-core Relational Schema and Indexing for Integration Boundaries: Verify Locked Version Drizzle Orm Repository

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Verify the locked version of drizzle-orm in the repository resolution artifact prior to using or modifying schema APIs.
- **R-DRIZZLE-002** MUST: Ensure composite index column ordering places equality filters first followed by range or sort columns such as creation timestamps.
- **R-DRIZZLE-003** SHOULD: Group related schema definitions by domain boundary while sharing common identifier conventions across tables.

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