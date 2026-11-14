---
version: 2
name: cross-cutting-service-functions-not-process-raw-c11d
description: "Zod Schema-Based Input Validation at Service Boundaries: Service Functions Not Process Raw Unparsed"
metadata:
  actual:
    adr_id: c11dc998-4061-4c65-b855-0fd1b9a91719
    status: accepted
    candidate_group_id: 06de24a5-bd6e-4387-bd5c-4c5c768f6863
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Zod Schema-Based Input Validation at Service Boundaries: Service Functions Not Process Raw Unparsed

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST_NOT: Service functions MUST NOT process raw unparsed arguments directly in database operations or business transactions.

### Verify

```bash
# Discover and execute the build/type-checking scripts from the package manifest
# Discover and execute the test execution script from the package manifest
# Discover and execute the static analysis or lint script from the package manifest
```

**Accept when:**
- All service endpoints and agent tools parse incoming arguments using designated schemas prior to business logic execution.
- Automated verification confirms that malformed or out-of-specification input arguments are rejected at the service boundary.
- Static type verification and automated tests pass without schema or validation discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new or modified service entry points must parse inputs against defined schemas.
</enforcement>