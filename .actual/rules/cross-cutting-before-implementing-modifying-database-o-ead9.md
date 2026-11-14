---
version: 2
name: cross-cutting-before-implementing-modifying-database-o-ead9
description: "Centralized Relational Data Access with @openstatus/db: Before Implementing Modifying Database Operations Developers"
metadata:
  actual:
    adr_id: ead91cc1-af56-4089-a9a4-c7556acf2cec
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

# Centralized Relational Data Access with @openstatus/db: Before Implementing Modifying Database Operations Developers

Scope: applies to `packages/api/src/auth/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all backend API procedures, route handlers, and authentication helpers executing data queries or mutations against domain entities, and any data access logic requiring multi-tenant scoping by workspace or user ownership.

### Rules

- **R-DB-001** MUST: Before implementing or modifying database operations, developers MUST inspect the workspace dependency manifest and repository lock artifact to discover and resolve the exact locked version of all database dependencies.

### Verify

```bash
# Discover and run the project static type analysis verification script from the repository manifest to verify all queries comply with database schema definitions.
# Execute the repository test validation command discovered from the project configuration to verify multi-tenant isolation and query correctness.
# Run the project structural linting check discovered from the build configuration to verify that no direct database connection initializations bypass the centralized database package.
```

**Accept when:**
- Static type analysis passes with zero type errors across all database query sites and schema references.
- Automated test suites pass successfully, verifying that all tenant-scoped queries isolate data by workspace identifier.
- Static inspection confirms that no procedure routes instantiate private database connection pools outside the centralized database package.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>