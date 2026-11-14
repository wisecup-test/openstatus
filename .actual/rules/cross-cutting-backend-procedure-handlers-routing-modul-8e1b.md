---
version: 2
name: cross-cutting-backend-procedure-handlers-routing-modul-8e1b
description: "Centralized Relational Data Access with @openstatus/db: Backend Procedure Handlers Routing Modules Execute"
metadata:
  actual:
    adr_id: 8e1ba7f8-a5d3-40db-863f-987a017b6486
    status: accepted
    candidate_group_id: 0e1a37eb-0f9b-4ab0-9e8c-680ac9ec4729
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/auth/**
      - src/router/**
---

# Centralized Relational Data Access with @openstatus/db: Backend Procedure Handlers Routing Modules Execute

Scope: applies to `packages/api/src/auth/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: All backend procedure handlers and routing modules MUST execute database queries, entity mutations, and relational traversals exclusively through database clients and schema definitions provided by @openstatus/db.

### Verify

```bash
# Discover and run static type analysis and tests from the project manifest/configuration
# Example verification steps:
npx tsc --noEmit
npm test
```

**Accept when:**
- Static type analysis passes with zero type errors across all database query sites and schema references.
- Automated test suites pass successfully, verifying that all tenant-scoped queries isolate data by workspace identifier.
- Static inspection confirms that no procedure routes instantiate private database connection pools outside the centralized database package.

<enforcement>
Claude Code MUST NOT skip or defer verification. All queries, mutations, and database clients must route exclusively through @openstatus/db with proper workspace scoping and type safety verified via static analysis and test suites.
</enforcement>