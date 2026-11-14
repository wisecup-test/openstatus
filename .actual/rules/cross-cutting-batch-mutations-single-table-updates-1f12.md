---
version: 2
name: cross-cutting-batch-mutations-single-table-updates-1f12
description: "Centralized Relational Data Access with @openstatus/db: Batch Mutations Single Table Updates Use"
metadata:
  actual:
    adr_id: 1f12c24d-ce92-4a82-84ba-b9f6f17c9d2f
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

# Centralized Relational Data Access with @openstatus/db: Batch Mutations Single Table Updates Use

Scope: applies to `packages/api/src/auth/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all backend API procedures, route handlers, and authentication helpers executing data queries or mutations against domain entities, and any data access logic requiring multi-tenant scoping by workspace or user ownership.

### Rules

- **R-DB-001** MAY: Batch mutations and single-table updates MAY use SQL query builder expressions when declarative relational queries do not natively support the required mutation pattern.

### Verify

```bash
# Discover and run static type analysis verification from the repository manifest
# Execute repository test validation command for multi-tenant isolation and query correctness
# Run project structural linting check to verify no direct database connection initializations bypass the centralized database package
```

**Accept when:**
- Static type analysis passes with zero type errors across all database query sites and schema references.
- Automated test suites pass successfully, verifying that all tenant-scoped queries isolate data by workspace identifier.
- Static inspection confirms that no procedure routes instantiate private database connection pools outside the centralized database package.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated continuous integration pipelines, peer code reviews, and static analysis lint checks is mandatory.
</enforcement>