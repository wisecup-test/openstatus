---
version: 2
name: cross-cutting-internal-schema-specific-validation-logi-e8f9
description: "Adoption of @openstatus/utils for Domain Schema Utilities: Internal Schema Specific Validation Logic That"
metadata:
  actual:
    adr_id: e8f9f265-01a0-40f3-a53b-7d91b2681515
    status: accepted
    candidate_group_id: 878f0b23-382e-42a3-b480-16f01e24f6b7
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/**
---

# Adoption of @openstatus/utils for Domain Schema Utilities: Internal Schema Specific Validation Logic That

Scope: applies to `packages/db/src/schema/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** SHOULD_NOT: Internal schema-specific validation logic that has no reuse potential outside a single domain entity SHOULD NOT be exported to @openstatus/utils.

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
Claude Code MUST NOT skip or defer verification. All changes affecting schema definitions or shared utilities must be verified via continuous integration type checking, linting workflows, and mandatory peer code review.
</enforcement>