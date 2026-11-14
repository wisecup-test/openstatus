---
version: 2
name: cross-cutting-before-implementing-updating-modules-tha-53c1
description: "Adoption of @openstatus/utils for Domain Schema Utilities: Before Implementing Updating Modules That Depend"
metadata:
  actual:
    adr_id: 53c1df19-aa03-4fe0-a626-efcfc59bfe74
    status: accepted
    candidate_group_id: 878f0b23-382e-42a3-b480-16f01e24f6b7
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/utils for Domain Schema Utilities: Before Implementing Updating Modules That Depend

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all database schema definitions, constants, and validation modules requiring shared utility functions or common validation primitives, and domain packages defining cross-cutting constants or schemas across domain entities.

### Rules

- **R-OPENSTATUS-001** MUST: Before implementing or updating modules that depend on @openstatus/utils, consumers MUST discover the repository dependency resolution artifact to verify the exact resolved version and ensure compatibility with documented public interfaces.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>