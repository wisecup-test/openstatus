---
version: 2
name: cross-cutting-procedure-definitions-declare-schemas-ex-7c89
description: "Zod Runtime Schema Validation for API Procedure Boundaries: Procedure Definitions Declare Schemas Explicit Primitive"
metadata:
  actual:
    adr_id: 7c899c81-e393-4913-abc6-80feee8a8158
    status: accepted
    candidate_group_id: 0cbb6056-fd7d-4539-b46b-da94fc5fa10f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Zod Runtime Schema Validation for API Procedure Boundaries: Procedure Definitions Declare Schemas Explicit Primitive

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all public and protected procedure definitions accepting parameters from external callers, and runtime parsing of dynamic configuration objects and deserialized entity attributes.

### Rules

- **R-ZOD-001** MUST: Procedure definitions MUST declare schemas using explicit primitive types, bounds, enumerations, or structural objects, and MUST NOT accept unconstrained or untyped input objects.

### Verify

```bash
# Discover and execute the repository build and type verification script from the project manifest
# Discover and execute the repository automated test runner
# Discover and run the project static analysis linter
```

**Accept when:**
- All public and protected procedure definitions declare explicit input validation schemas prior to invoking service handlers.
- Type checking and automated test suites confirm schema compilation and successful rejection of malformed payloads.
- Boundary parser rejections return structured client errors without propagating unvalidated state to domain functions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds will fail upon unvalidated input parameters or missing procedure schemas.
</enforcement>