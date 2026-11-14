---
version: 2
name: cross-cutting-indexes-uniqueness-constraints-entity-at-a101
description: "Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Indexes Uniqueness Constraints Entity Attributes Declared"
metadata:
  actual:
    adr_id: a10147e5-31fb-4e23-a606-d21ab50f285d
    status: accepted
    candidate_group_id: 2194cfcc-004d-497d-b4b1-2c95a06ad76e
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of drizzle-orm and drizzle-orm/sqlite-core for Database Schema and Index Definition: Indexes Uniqueness Constraints Entity Attributes Declared

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DRIZZLE-001** MUST: Indexes and uniqueness constraints on entity attributes MUST be declared via the index, uniqueIndex, or unique API builders directly chained within the table schema definition.

### Verify

```bash
# Discover and run typecheck verification task from the manifest
# Discover and run database migration or schema check command configured in project scripts
```

**Accept when:**
- The project typecheck verification passes with zero type diagnostics across all schema modules.
- All entity table and index definitions successfully generate valid dialect DDL without schema validation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>