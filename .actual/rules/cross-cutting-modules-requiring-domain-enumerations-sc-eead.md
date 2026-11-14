---
version: 2
name: cross-cutting-modules-requiring-domain-enumerations-sc-eead
description: "Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Modules Requiring Domain Enumerations Scheduling Periodicity"
metadata:
  actual:
    adr_id: eead5bd8-1fd4-4c3a-9cc9-780e8373d2b2
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

# Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Modules Requiring Domain Enumerations Scheduling Periodicity

Scope: applies to `apps/dashboard/src/components/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for validation schemas, data retrieval utilities, user interface components, and application layers referencing shared domain enumerations or database configuration constants.

### Rules

- **R-OPENSTATUS-001** MUST: Modules requiring domain enumerations, scheduling periodicity constants, or database-aligned configuration values MUST import them directly from @openstatus/db/src/schema/constants.

### Verify

```bash
# Discover and run project static analysis and linting scripts to verify no local duplicates of domain constants are defined
npm run lint || pnpm lint || yarn lint

# Discover and execute type check and test suites across workspace packages
npm run test || pnpm test || yarn test
```

**Accept when:**
- Static type analysis passes across all workspace packages without type errors or resolution failures referencing the schema constants module.
- Automated test suites confirm schema validation correctly accepts canonical enumerations and rejects invalid inputs.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>