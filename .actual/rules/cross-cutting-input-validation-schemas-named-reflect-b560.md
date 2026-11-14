---
version: 2
name: cross-cutting-input-validation-schemas-named-reflect-b560
description: "Zod Schema-Based Input Validation at Service Boundaries: Input Validation Schemas Named Reflect Corresponding"
metadata:
  actual:
    adr_id: b560c3a1-215e-4353-97a6-59cb975c0a8d
    status: accepted
    candidate_group_id: 06de24a5-bd6e-4387-bd5c-4c5c768f6863
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Zod Schema-Based Input Validation at Service Boundaries: Input Validation Schemas Named Reflect Corresponding

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-VAL-001** SHOULD: Input validation schemas SHOULD be named to reflect the corresponding service operation and be co-located or imported from designated schema definitions.

### Verify

```bash
# Discover the repository build, type-checking, test, and lint scripts from the package manifest and execute them
npm run build --if-present
npm run test --if-present
npm run lint --if-present
```

**Accept when:**
- All service endpoints and agent tools parse incoming arguments using designated schemas prior to business logic execution.
- Automated verification confirms that malformed or out-of-specification input arguments are rejected at the service boundary.
- Static type verification and automated tests pass without schema or validation discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>