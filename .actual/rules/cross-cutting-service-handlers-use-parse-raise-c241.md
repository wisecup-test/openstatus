---
version: 2
name: cross-cutting-service-handlers-use-parse-raise-c241
description: "Zod Schema-Based Input Validation at Service Boundaries: Service Handlers Use Parse Raise Validation"
metadata:
  actual:
    adr_id: c241a847-877e-4094-a058-8539b5f90487
    status: accepted
    candidate_group_id: 06de24a5-bd6e-4387-bd5c-4c5c768f6863
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Zod Schema-Based Input Validation at Service Boundaries: Service Handlers Use Parse Raise Validation

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD: Service handlers SHOULD use parse to raise validation errors immediately on malformed input, or safeParse when conditional handling of validation results is required.

### Verify

```bash
# Discover and run the repository build and type-checking scripts
# Discover and run the repository test execution script
# Discover and run the repository static analysis or lint script
```

**Accept when:**
- All service endpoints and agent tools parse incoming arguments using designated schemas prior to business logic execution.
- Automated verification confirms that malformed or out-of-specification input arguments are rejected at the service boundary.
- Static type verification and automated tests pass without schema or validation discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new or modified service entry points must parse inputs against defined schemas.
</enforcement>