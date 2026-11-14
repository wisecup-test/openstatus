---
version: 2
name: cross-cutting-schema-definitions-define-specific-const-1780
description: "Zod Schema Definition and Runtime Validation Library Adoption: Schema Definitions Define Specific Constraint Rules"
metadata:
  actual:
    adr_id: 1780e40e-dd1f-49f4-bf3a-ba96b937f5e9
    status: accepted
    candidate_group_id: c1f28a38-ae70-4493-874a-5738669cad24
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation Library Adoption: Schema Definitions Define Specific Constraint Rules

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Schema definitions SHOULD define specific constraint rules including string formats, numeric bounds, and enumerated value sets directly within the schema declaration rather than through downstream procedural logic.

### Verify

```bash
# Discover and execute the project test runner script defined in the repository dependency manifest to verify schema validation test suites pass.
# Discover and execute the static type verification script declared in the repository workspace configuration to ensure all schema-inferred types compile without errors.
# Discover and execute the project linter and format validation scripts to verify adherence to validation standards across all route and adapter files.
```

**Accept when:**
- All schema validation test suites pass successfully across boundary adapters and route handlers.
- Static type analysis passes with zero diagnostic errors across all modules defining or consuming schema-inferred types.
- Runtime input parsing reliably rejects malformed boundary payloads and permits valid payloads across all integration entry points.

<enforcement>
Claude Code MUST NOT skip or defer verification. All boundary entry points and integration handlers must use standardized schema validation, verified by automated continuous integration test and static type verification suites.
</enforcement>