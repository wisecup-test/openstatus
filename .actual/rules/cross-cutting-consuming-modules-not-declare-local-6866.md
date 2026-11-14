---
version: 2
name: cross-cutting-consuming-modules-not-declare-local-6866
description: "Adoption of @openstatus/db Module for Shared Schema and Persistence: Consuming Modules Not Declare Local Database"
metadata:
  actual:
    adr_id: 6866c112-7ee7-407a-9289-dafff7c4953e
    status: accepted
    candidate_group_id: 5edccb20-8590-46b8-98ed-d0c4028cd2d5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/db Module for Shared Schema and Persistence: Consuming Modules Not Declare Local Database

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: Consuming modules MUST NOT declare local database schema duplicates, bypass @openstatus/db/src/schema, or initialize independent raw connection pools outside the centralized database module.

### Verify

```bash
# Discover workspace dependency manifest and execute type-checking and linting suites
# (Command depends on the discovered package manager, e.g. pnpm check or npm run build)
```

**Accept when:**
- All imports of @openstatus/db and @openstatus/db/src/schema compile without unresolved module or type errors across the workspace.
- No route or application file declares duplicate local database entity schemas or independent raw connection instances.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>