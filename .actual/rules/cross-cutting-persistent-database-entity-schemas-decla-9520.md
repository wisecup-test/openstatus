---
version: 2
name: cross-cutting-persistent-database-entity-schemas-decla-9520
description: "Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Persistent Database Entity Schemas Declared Drizzle"
metadata:
  actual:
    adr_id: 952082ba-088d-4ff1-a237-6a0d77a1c24b
    status: accepted
    candidate_group_id: 2194cfcc-004d-497d-b4b1-2c95a06ad76e
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Persistent Database Entity Schemas Declared Drizzle

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: All persistent database entity schemas MUST be declared using drizzle-orm and its SQLite core dialect module drizzle-orm/sqlite-core.

### Verify

```bash
# Discover and execute the project typecheck verification task from the manifest
# Discover and execute the database migration or schema check command configured in project scripts
```

**Accept when:**
- The project typecheck verification passes with zero type diagnostics across all schema modules.
- All entity table and index definitions successfully generate valid dialect DDL without schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>