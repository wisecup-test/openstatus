---
version: 2
name: cross-cutting-input-validation-schemas-define-explicit-1389
description: "Zod Schema-Based Input Validation at Service Boundaries: Input Validation Schemas Define Explicit Structural"
metadata:
  actual:
    adr_id: 1389a618-e87a-4b8c-96cb-82bf28eec715
    status: accepted
    candidate_group_id: 06de24a5-bd6e-4387-bd5c-4c5c768f6863
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Zod Schema-Based Input Validation at Service Boundaries: Input Validation Schemas Define Explicit Structural

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Input validation schemas MUST define explicit structural constraints and types for all expected fields using Zod object definitions and schema primitives.

### Verify

```bash
# Discover and execute the build/type-checking scripts from the package manifest
# Discover and execute the test suite to verify boundary validation behavior
# Discover and execute static analysis/lint scripts to verify no service handler bypasses schema parsing
```

**Accept when:**
- All service endpoints and agent tools parse incoming arguments using designated schemas prior to business logic execution.
- Automated verification confirms that malformed or out-of-specification input arguments are rejected at the service boundary.
- Static type verification and automated tests pass without schema or validation discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new or modified service entry points must parse inputs against defined Zod schemas.
</enforcement>