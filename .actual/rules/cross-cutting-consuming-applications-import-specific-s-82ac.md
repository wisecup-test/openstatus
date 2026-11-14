---
version: 2
name: cross-cutting-consuming-applications-import-specific-s-82ac
description: "Adoption of @openstatus/db Module for Shared Schema and Persistence: Consuming Applications Import Specific Subpath Exports"
metadata:
  actual:
    adr_id: 82ac4908-b2c8-4cc1-a48a-9287b221fd29
    status: accepted
    candidate_group_id: 5edccb20-8590-46b8-98ed-d0c4028cd2d5
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of @openstatus/db Module for Shared Schema and Persistence: Consuming Applications Import Specific Subpath Exports

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-rendered endpoints, route handlers, metadata generation modules, workspace packages, and applications that execute database queries or consume shared entity schema definitions.

### Rules

- **R-DB-001** MAY: Consuming applications MAY import specific subpath exports from @openstatus/db/src/schema when only type definitions or partial table schemas are needed for data transformations.

### Verify

```bash
# Discover the workspace dependency manifest and execute project type-checking and linting suites across all consuming applications
# Locate and run the automated test suite for applications consuming @openstatus/db
```

**Accept when:**
- All imports of @openstatus/db and @openstatus/db/src/schema compile without unresolved module or type errors across the workspace.
- No route or application file declares duplicate local database entity schemas or independent raw connection instances.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database interactions must import canonical schema definitions exclusively from @openstatus/db.
</enforcement>