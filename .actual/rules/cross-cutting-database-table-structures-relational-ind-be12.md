---
version: 2
name: cross-cutting-database-table-structures-relational-ind-be12
description: "Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Database Table Structures Relational Index Definitions"
metadata:
  actual:
    adr_id: be126e88-3ed0-47dd-8bc9-30bacd2913e6
    status: accepted
    candidate_group_id: 2194cfcc-004d-497d-b4b1-2c95a06ad76e
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Database Table Structures Relational Index Definitions

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST_NOT: Database table structures and relational index definitions MUST_NOT be declared through unmanaged raw SQL statements when a corresponding declarative schema API is supported.

### Verify

```bash
# Discover the project typecheck verification task from the manifest and execute it
# Discover the database migration or schema check command configured in the project scripts and execute it
```

**Accept when:**
- The project typecheck verification passes with zero type diagnostics across all schema modules.
- All entity table and index definitions successfully generate valid dialect DDL without schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>