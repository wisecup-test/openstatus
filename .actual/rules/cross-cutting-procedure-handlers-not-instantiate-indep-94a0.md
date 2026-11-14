---
version: 2
name: cross-cutting-procedure-handlers-not-instantiate-indep-94a0
description: "Centralized Relational Data Access with @openstatus/db: Procedure Handlers Not Instantiate Independent Database"
metadata:
  actual:
    adr_id: 94a08d3a-a45f-43b1-ab84-3abdc251f35d
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

# Centralized Relational Data Access with @openstatus/db: Procedure Handlers Not Instantiate Independent Database

Scope: applies to `packages/api/src/auth/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all backend API procedures, route handlers, and authentication helpers executing data queries or mutations against domain entities, and any data access logic requiring multi-tenant scoping by workspace or user ownership.

### Rules

- **R-DB-001** MUST_NOT: Procedure handlers MUST NOT instantiate independent database connection pools or issue raw unescaped SQL strings outside the centralized database abstraction.

### Verify

```bash
# Discover and run the project static type analysis verification script from the repository manifest
# Execute the repository test validation command discovered from the project configuration
# Run the project structural linting check discovered from the build configuration
```

**Accept when:**
- Static type analysis passes with zero type errors across all database query sites and schema references.
- Automated test suites pass successfully, verifying that all tenant-scoped queries isolate data by workspace identifier.
- Static inspection confirms that no procedure routes instantiate private database connection pools outside the centralized database package.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration pipelines, peer code reviews, and static analysis lint checks.
</enforcement>