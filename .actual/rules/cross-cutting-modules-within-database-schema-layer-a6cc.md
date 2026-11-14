---
version: 2
name: cross-cutting-modules-within-database-schema-layer-a6cc
description: "Adoption of @openstatus/utils for Domain Schema Utilities: Modules Within Database Schema Layer Not"
metadata:
  actual:
    adr_id: a6ccc020-0507-4d59-85ca-e8ee3e82ad2b
    status: accepted
    candidate_group_id: 878f0b23-382e-42a3-b480-16f01e24f6b7
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/utils for Domain Schema Utilities: Modules Within Database Schema Layer Not

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all database schema definitions, constants, and validation modules requiring shared utility functions or common validation primitives, as well as domain packages defining cross-cutting constants or schemas across domain entities.

### Rules

- **R-DB-001** MUST_NOT: Modules within the database schema layer MUST NOT re-implement local copies of shared URL validation schemas, formatting helpers, or common domain constants provided by @openstatus/utils.

### Verify

```bash
# Discover and execute project test suite across database schema packages
# Discover repository static analysis and type checking scripts, then execute them against schema modules
# Discover dependency analysis scripts to verify proper workspace linking for @openstatus/utils
```

**Accept when:**
- All database schema and validation modules successfully compile and pass static analysis with all shared utility imports resolved from @openstatus/utils.
- No duplicate validation primitives or redundant domain helper implementations exist across schema modules.
- All automated test suites for schema and validation logic pass with zero errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing redundant schema utilities or circumventing @openstatus/utils are blocked from merging.
</enforcement>