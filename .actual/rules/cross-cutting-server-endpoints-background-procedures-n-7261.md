---
version: 2
name: cross-cutting-server-endpoints-background-procedures-n-7261
description: "Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Server Endpoints Background Procedures Not Instantiate"
metadata:
  actual:
    adr_id: 7261a056-3e01-46f5-9e32-577f253fc9cf
    status: accepted
    candidate_group_id: 8a25c2df-024a-4181-8d98-527a4ff68ed7
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/trpc/**
---

# Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Server Endpoints Background Procedures Not Instantiate

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: Server endpoints and background procedures MUST NOT instantiate independent database connection pools or redefine relational schemas locally outside of @openstatus/db.

### Verify

```bash
# Discover and run the repository static analysis and type verification scripts
npm run typecheck
# Run the project automated test suite
npm test
```

**Accept when:**
- Static type analysis passes without import or typing errors across all consuming application modules.
- All automated integration and unit test suites referencing shared database schemas pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing direct database driver instantiations or unapproved local schema definitions are blocked from merging until corrected.
</enforcement>