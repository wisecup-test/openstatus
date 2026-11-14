---
version: 2
name: cross-cutting-form-validation-logic-integrate-zod-edd5
description: "Zod Schema Validation for Form and Component Input Boundaries: Form Validation Logic Integrate Zod Schemas"
metadata:
  actual:
    adr_id: edd5b122-41e4-44b3-a038-a6396ab205a4
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

# Zod Schema Validation for Form and Component Input Boundaries: Form Validation Logic Integrate Zod Schemas

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/**`, `apps/dashboard/src/scripts/**`.

These rules are ALWAYS ACTIVE for all user-facing dashboard forms, input dialogs, configuration panels capturing user input, and deserialization/hydration boundaries for client-persisted session data and search parameter caches.

### Rules

- **R-ZOD-001** MUST: Form validation logic MUST integrate Zod schemas into form management hooks using the dedicated Zod resolver binding rather than implementing manual field-level validation callbacks.

### Verify

```bash
# Discover and execute the project static analysis scripts to verify that all form components bind corresponding Zod schemas without type or lint errors.
# Discover and execute the test runner suite to validate that input schemas reject malformed values and accept compliant payloads across component test fixtures.
```

**Accept when:**
- All user-facing form components bind an explicit Zod validation schema through the resolver binding without unvalidated input paths.
- Static type checking and automated test runner suites pass with zero validation or schema mismatch errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>