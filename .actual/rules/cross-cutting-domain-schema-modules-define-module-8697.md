---
version: 2
name: cross-cutting-domain-schema-modules-define-module-8697
description: "Adoption of @openstatus/utils for Domain Schema Utilities: Domain Schema Modules Define Module Local"
metadata:
  actual:
    adr_id: 86974903-3089-48f0-b6a9-0dc97b2d11ca
    status: accepted
    candidate_group_id: 878f0b23-382e-42a3-b480-16f01e24f6b7
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/utils for Domain Schema Utilities: Domain Schema Modules Define Module Local

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MAY: Domain schema modules MAY define module-local constants when those constants are strictly scoped to a single schema table and do not represent shared utility abstractions.

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