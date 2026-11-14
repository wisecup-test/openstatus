---
version: 2
name: cross-cutting-form-component-types-inferred-directly-46dd
description: "Zod Schema Validation for Form and Component Input Boundaries: Form Component Types Inferred Directly Zod"
metadata:
  actual:
    adr_id: 46dddcf8-84af-4353-a018-2416f66568bc
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

# Zod Schema Validation for Form and Component Input Boundaries: Form Component Types Inferred Directly Zod

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/**`, `apps/dashboard/src/scripts/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Form component types SHOULD be inferred directly from Zod schemas using schema type inference utilities to maintain synchronization between runtime validation and static interface definitions.

### Verify

```bash
# Discover and execute the project static analysis scripts to verify that all form components bind corresponding Zod schemas without type or lint errors.
# Discover and execute the test runner suite to validate that input schemas reject malformed values and accept compliant payloads across component test fixtures.
```

**Accept when:**
- All user-facing form components bind an explicit Zod validation schema through the resolver binding without unvalidated input paths.
- Static type checking and automated test runner suites pass with zero validation or schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated continuous integration checks executing static analysis and type verification is mandatory.
</enforcement>