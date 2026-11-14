---
version: 2
name: cross-cutting-consuming-modules-validate-query-inputs-c155
description: "Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Consuming Modules Validate Query Inputs Outputs"
metadata:
  actual:
    adr_id: c15515a6-11bd-485a-8e65-3b6242c1c71e
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

# Adoption of @openstatus/db Shared Workspace Package for Centralized Data Access and Schemas: Consuming Modules Validate Query Inputs Outputs

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all server-side application route handlers, background execution entry points, and shared procedure callers requiring database access.

### Rules

- **R-DB-001** SHOULD: Consuming modules SHOULD validate query inputs and outputs against schemas exported by @openstatus/db.

### Verify

```bash
# Discover and run repository static analysis and type verification scripts
# Example: pnpm check-types / npm run typecheck

# Discover and run project automated test suite
# Example: pnpm test / npm test
```

**Accept when:**
- Static type analysis passes without import or typing errors across all consuming application modules.
- All automated integration and unit test suites referencing shared database schemas pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline executing typecheck and test verification suites, and peer code reviews.
</enforcement>