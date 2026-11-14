---
version: 2
name: cross-cutting-shared-validation-schemas-that-require-66a3
description: "Adoption of @openstatus/utils for Domain Schema Utilities: Shared Validation Schemas That Require Sanitization"
metadata:
  actual:
    adr_id: 66a3e15c-b3c0-4c18-84b3-6ce523f43b5d
    status: accepted
    candidate_group_id: 878f0b23-382e-42a3-b480-16f01e24f6b7
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/utils for Domain Schema Utilities: Shared Validation Schemas That Require Sanitization

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-UTIL-001** SHOULD: Shared validation schemas that require sanitization or URL safety SHOULD derive from common primitives exposed by @openstatus/utils.

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
Claude Code MUST NOT skip or defer verification. All database schema definitions and validation modules must be verified against centralized shared utility requirements.
</enforcement>