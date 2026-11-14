---
version: 2
name: cross-cutting-applications-requiring-access-relational-f2b9
description: "Adoption of @openstatus/db Module for Shared Schema and Persistence: Applications Requiring Access Relational Entities Persistence"
metadata:
  actual:
    adr_id: f2b9b19a-dce8-44d6-b411-679301b6450c
    status: accepted
    candidate_group_id: 5edccb20-8590-46b8-98ed-d0c4028cd2d5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/db Module for Shared Schema and Persistence: Applications Requiring Access Relational Entities Persistence

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-rendered endpoints, route handlers, metadata generation modules, workspace packages, and applications requiring access to persistent domain data.

### Rules

- **R-DB-001** MUST: Applications requiring access to relational entities or persistence operations MUST import database clients and schema definitions exclusively from the internal @openstatus/db and @openstatus/db/src/schema module.

### Verify

```bash
# Discover workspace dependency manifest and execute type-checking and linting suites
pnpm typecheck || npm run typecheck
pnpm lint || npm run lint
# Run automated test suites for consuming applications
pnpm test || npm run test
```

**Accept when:**
- All imports of @openstatus/db and @openstatus/db/src/schema compile without unresolved module or type errors across the workspace.
- No route or application file declares duplicate local database entity schemas or independent raw connection instances.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing workspace-wide type verification and import boundary linting, as well as peer code reviews.
</enforcement>