---
version: 2
name: cross-cutting-before-implementing-modifying-validation-17d0
description: "Zod Schema Validation for Form and Component Input Boundaries: Before Implementing Modifying Validation Schemas Developers"
metadata:
  actual:
    adr_id: 17d09932-8ab1-4568-9d27-7225da36ad18
    status: accepted
    candidate_group_id: df6eea8a-3ab7-4843-bbef-10688bb78119
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/components/**
      - src/lib/**
      - src/scripts/**
---

# Zod Schema Validation for Form and Component Input Boundaries: Before Implementing Modifying Validation Schemas Developers

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/**`, `apps/dashboard/src/scripts/**`.

These rules are ALWAYS ACTIVE for all user-facing dashboard forms, input dialogs, configuration panels capturing user input, and deserialization/hydration boundaries for client-persisted session data and search parameter caches.

### Rules

- **R-ZOD-001** MUST: Before implementing or modifying validation schemas, developers MUST inspect the repository dependency lock artifact to determine the exact resolved library version and verify API compatibility against that version's specification.
- **R-ZOD-002** MUST: Instantiate validation schemas at module scope rather than within component render functions to prevent unnecessary schema re-allocation on re-renders.
- **R-ZOD-003** MUST: Compose complex object schemas hierarchically using shared sub-schemas and domain constants to preserve cross-form consistency.

### Verify

```bash
# Discover and execute the project static analysis scripts to verify Zod schema bindings
# Discover and execute the test runner suite to validate schema acceptance/rejection
```

**Accept when:**
- All user-facing form components bind an explicit Zod validation schema through the resolver binding without unvalidated input paths.
- Static type checking and automated test runner suites pass with zero validation or schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis and automated test suites.
</enforcement>