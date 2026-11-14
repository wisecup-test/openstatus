---
version: 2
name: cross-cutting-validation-schemas-define-explicit-const-5fd0
description: "Zod Schema Validation for Form and Component Input Boundaries: Validation Schemas Define Explicit Constraints Scalar"
metadata:
  actual:
    adr_id: 5fd00222-e0fc-4fd0-ba72-e459239ee38c
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

# Zod Schema Validation for Form and Component Input Boundaries: Validation Schemas Define Explicit Constraints Scalar

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/**`, `apps/dashboard/src/scripts/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Validation schemas MUST define explicit constraints on scalar and structured fields, including allowed enumerations, required string lengths, and specialized formats, rather than permitting untyped or unconstrained input.

### Verify

```bash
# Discover and execute the project static analysis and test runner scripts to verify validation schema bindings
npm run lint || pnpm lint || yarn lint
npm test || pnpm test || yarn test
```

**Accept when:**
- All user-facing form components bind an explicit Zod validation schema through the resolver binding without unvalidated input paths.
- Static type checking and automated test runner suites pass with zero validation or schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new form inputs and external data decoders must incorporate corresponding Zod schemas, and pull requests containing unvalidated form inputs must be blocked.
</enforcement>