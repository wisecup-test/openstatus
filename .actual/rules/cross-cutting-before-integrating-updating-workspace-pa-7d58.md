---
version: 2
name: cross-cutting-before-integrating-updating-workspace-pa-7d58
description: "Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Before Integrating Updating Workspace Package Dependencies"
metadata:
  actual:
    adr_id: 7d58ca9d-36f7-4dde-99a0-e7f598b35d51
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

# Adoption of @openstatus/db Internal Package for Database Access and Schema Definitions: Before Integrating Updating Workspace Package Dependencies

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application route handlers, scheduled tasks, and domain utility modules that query relational storage or reference database entity types.

### Rules

- **R-DB-001** MUST: Before integrating or updating workspace package dependencies, consumers MUST inspect the repository lock artifact to verify the exact resolved dependency versions and confirm that exported package interfaces match the resolved artifacts.
- **R-DB-002** MUST: Ensure newly introduced database table schemas and relational models are defined within the shared database module and exported through public module subpaths before importing them into endpoint handlers.
- **R-DB-003** MUST: Verify that all required environment configuration values for database client initialization are validated prior to executing database queries.

### Verify

```bash
# Discover and execute project static analysis and type verification scripts
npm run check || pnpm check || yarn check || true

# Run repository test suite
npm test || pnpm test || yarn test || true
```

**Accept when:**
- All database access and schema imports resolve through @openstatus/db without unresolved module references.
- Static type verification and automated test suites execute with zero errors across all consuming modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database persistence logic and schema references must use the designated internal database package and pass workspace verification checks.
</enforcement>