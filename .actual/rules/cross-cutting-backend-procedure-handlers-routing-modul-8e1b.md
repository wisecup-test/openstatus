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

These rules are ALWAYS ACTIVE for all backend API procedures, route handlers, and authentication helpers executing data queries or mutations against domain entities, and any data access logic requiring multi-tenant scoping by workspace or user ownership.

### Rules

- **R-DB-001** MUST: All backend procedure handlers and routing modules MUST execute database queries, entity mutations, and relational traversals exclusively through database clients and schema definitions provided by @openstatus/db.

### Verify

```bash
# Discover and run the project static type analysis verification script from the repository manifest
npm run typecheck

# Execute the repository test validation command discovered from the project configuration
npm test

# Run the project structural linting check discovered from the build configuration
npm run lint
```

**Accept when:**
- Static type analysis passes with zero type errors across all database query sites and schema references.
- Automated test suites pass successfully, verifying that all tenant-scoped queries isolate data by workspace identifier.
- Static inspection confirms that no procedure routes instantiate private database connection pools outside the centralized database package.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>