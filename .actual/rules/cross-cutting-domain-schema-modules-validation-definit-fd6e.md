---
version: 2
name: cross-cutting-domain-schema-modules-validation-definit-fd6e
description: "Adoption of @openstatus/utils for Domain Schema Utilities: Domain Schema Modules Validation Definitions Import"
metadata:
  actual:
    adr_id: fd6e5af6-4091-4587-8b22-c94dd9fecab7
    status: accepted
    candidate_group_id: 878f0b23-382e-42a3-b480-16f01e24f6b7
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/utils for Domain Schema Utilities: Domain Schema Modules Validation Definitions Import

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for database schema definitions, constants, and validation modules requiring shared utility functions or common validation primitives, as well as domain packages defining cross-cutting constants or schemas across domain entities.

### Rules

- **R-SCHEMA-001** MUST: Domain schema modules and schema validation definitions MUST import shared utility primitives, validation helpers, and cross-cutting constants from @openstatus/utils.

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