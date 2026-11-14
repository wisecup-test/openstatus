---
version: 2
name: cross-cutting-application-components-data-utilities-no-e9f6
description: "Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Application Components Data Utilities Not Declare"
metadata:
  actual:
    adr_id: e9f61bf5-70dc-41a6-ad54-c0f9f0e14cc5
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

# Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Application Components Data Utilities Not Declare

Scope: applies to `apps/dashboard/src/components/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for validation schemas, data retrieval utilities, and user interface components referencing shared domain enumerations or database configuration constants.

### Rules

- **R-OC-001** MUST_NOT: Application components and data utilities MUST NOT declare local duplicate literals or re-export local shadows of constants provided by @openstatus/db/src/schema/constants.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify no local duplicates of domain constants are defined in consumer modules.
# Discover and execute the type check and test suites across all workspace packages to verify imported constants satisfy interface contracts.
```

**Accept when:**
- Static type analysis passes across all workspace packages without type errors or resolution failures referencing the schema constants module.
- Automated test suites confirm schema validation correctly accepts canonical enumerations and rejects invalid inputs.

<enforcement>
Claude Code MUST NOT skip or defer verification. All additions of domain enumerations and validation schemas are audited by automated continuous integration checks and peer code review.
</enforcement>