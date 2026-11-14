---
version: 2
name: cross-cutting-application-services-server-side-endpoin-043a
description: "Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Application Services Server Side Endpoints Requiring"
metadata:
  actual:
    adr_id: 043a1ae4-2012-41ec-b9ca-37f80610b7dd
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

# Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Application Services Server Side Endpoints Requiring

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all server-side application route handlers, background execution entry points, and shared procedure callers requiring database access or relational schema validation.

### Rules

- **R-DB-001** MUST: All application services and server-side endpoints requiring relational persistence MUST source database client instances and data models exclusively from the internal @openstatus/db package.

### Verify

```bash
# Discover and run the repository static analysis and type verification scripts to validate that all database imports resolve to @openstatus/db.
# Discover and run the project automated test suite to ensure server routes querying the shared database package execute without regressions.
```

**Accept when:**
- Static type analysis passes without import or typing errors across all consuming application modules.
- All automated integration and unit test suites referencing shared database schemas pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>