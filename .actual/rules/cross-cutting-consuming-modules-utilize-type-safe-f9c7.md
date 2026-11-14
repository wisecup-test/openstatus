---
version: 2
name: cross-cutting-consuming-modules-utilize-type-safe-f9c7
description: "Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Consuming Modules Utilize Type Safe Query"
metadata:
  actual:
    adr_id: f9c78f29-8d8d-48f8-b9b2-84ea0bed59db
    status: accepted
    candidate_group_id: 6a59cfe8-1e88-4c01-871a-1db91c9d4ce9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/lib/**
---

# Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Consuming Modules Utilize Type Safe Query

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** SHOULD: Consuming modules SHOULD utilize type-safe query builder operations provided by the shared database client rather than authoring raw, untyped query strings.

### Verify

```bash
# Discover and execute project static analysis and type verification scripts
# Example: pnpm --filter @openstatus/db check (discover actual tool and command from repo)
npm run typecheck
npm test
```

**Accept when:**
- All database access and schema imports resolve through @openstatus/db without unresolved module references.
- Static type verification and automated test suites execute with zero errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>