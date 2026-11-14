---
version: 2
name: cross-cutting-validation-schemas-defining-periodicity-bbe3
description: "Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Validation Schemas Defining Periodicity Boundary Constraints"
metadata:
  actual:
    adr_id: bbe35e09-4e40-454d-90aa-8611bd23812e
    status: accepted
    candidate_group_id: 845ebcd2-20cc-4558-8366-edf050da5222
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/**
      - src/data/**
---

# Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Validation Schemas Defining Periodicity Boundary Constraints

Scope: applies to `apps/dashboard/src/components/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for validation schemas, data retrieval utilities, and user interface components referencing shared domain enumerations or database configuration constants.

### Rules

- **R-DOMAIN-001** MUST: Validation schemas defining periodicity or boundary constraints MUST derive their allowed values directly from exported enumerations in @openstatus/db/src/schema/constants.

### Verify

```bash
# Discover and run the project static analysis, linting, type check, and test scripts across all workspace packages
npm run lint
npm run typecheck
npm run test
```

**Accept when:**
- Static type analysis passes across all workspace packages without type errors or resolution failures referencing the schema constants module.
- Automated test suites confirm schema validation correctly accepts canonical enumerations and rejects invalid inputs.
- No local duplicates of domain constants are defined in consumer modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. All additions of domain enumerations and validation schemas must be verified by automated continuous integration checks and peer code review.
</enforcement>