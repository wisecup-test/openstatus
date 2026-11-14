---
version: 2
name: cross-cutting-application-modules-import-schema-defini-be20
description: "Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Application Modules Import Schema Definitions Query"
metadata:
  actual:
    adr_id: be20b8ad-42a5-471c-b38a-a2498a7880eb
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

# Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Application Modules Import Schema Definitions Query

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST: Application modules MUST import schema definitions and query builders via official module entry points provided by @openstatus/db.

### Verify

```bash
# Discover and run the repository static analysis and type verification scripts
npx tsc --noEmit
# Discover and run the project automated test suite
npm test
```

**Accept when:**
- Static type analysis passes without import or typing errors across all consuming application modules.
- All automated integration and unit test suites referencing shared database schemas pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>