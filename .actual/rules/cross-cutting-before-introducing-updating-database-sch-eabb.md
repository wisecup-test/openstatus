---
version: 2
name: cross-cutting-before-introducing-updating-database-sch-eabb
description: "Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Before Introducing Updating Database Schema Declarations"
metadata:
  actual:
    adr_id: eabb6728-97a9-4e56-be28-b251c17387c6
    status: accepted
    candidate_group_id: 2194cfcc-004d-497d-b4b1-2c95a06ad76e
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Before Introducing Updating Database Schema Declarations

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRZ-001** MUST: Before introducing or updating database schema declarations, inspect the repository dependency resolution lock artifact to identify the exact resolved version of the database ORM and verify API compatibility against that specific version.

### Verify

```bash
# Discover and run project typecheck verification
# Discover and run database migration or schema check command
```

**Accept when:**
- The project typecheck verification passes with zero type diagnostics across all schema modules.
- All entity table and index definitions successfully generate valid dialect DDL without schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All persistence schema declarations must be statically verified against the resolved lock artifact and project typecheck / schema check pipelines.
</enforcement>