---
version: 2
name: cross-cutting-multi-tenant-table-definitions-declare-79e9
description: "Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Multi Tenant Table Definitions Declare Explicit"
metadata:
  actual:
    adr_id: 79e9472c-013c-4a1d-a467-8b51fbe000b7
    status: accepted
    candidate_group_id: 2194cfcc-004d-497d-b4b1-2c95a06ad76e
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Multi Tenant Table Definitions Declare Explicit

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** SHOULD: Multi-tenant table definitions SHOULD declare explicit indexes on tenant foreign key identifiers to maintain consistent query filtering performance.

### Verify

```bash
# Discover and execute the project typecheck verification task from the manifest
# Discover and execute the database migration or schema check command
```

**Accept when:**
- The project typecheck verification passes with zero type diagnostics across all schema modules.
- All entity table and index definitions successfully generate valid dialect DDL without schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines and peer code reviews enforce these requirements.
</enforcement>