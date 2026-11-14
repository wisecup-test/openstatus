---
version: 2
name: cross-cutting-presentation-layers-visualization-charts-5200
description: "Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Presentation Layers Visualization Charts Consume Domain"
metadata:
  actual:
    adr_id: 520082e0-1adf-421b-a474-5d54c660d9aa
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

# Adoption of @openstatus/db/src/schema/constants for Centralized Domain Enumerations: Presentation Layers Visualization Charts Consume Domain

Scope: applies to `apps/dashboard/src/components/**`, `apps/dashboard/src/data/**`.

These rules are ALWAYS ACTIVE for validation schemas, data retrieval utilities, user interface components, and application layers referencing shared domain enumerations, scheduling intervals, regional metadata, or database configuration constants.

### Rules

- **R-CENT-001** SHOULD: Presentation layers and visualization charts SHOULD consume domain status constants via `@openstatus/db/src/schema/constants` rather than hardcoding presentation-specific equivalents.

### Verify

```bash
# Discover and run the project static analysis, type check, and linting scripts
# to verify no local duplicates of domain constants are defined in consumer modules.
# Example: pnpm check-types || npm run lint || yarn typecheck
```

**Accept when:**
- Static type analysis passes across all workspace packages without type errors or resolution failures referencing the schema constants module.
- Automated test suites confirm schema validation correctly accepts canonical enumerations and rejects invalid inputs.

<enforcement>
Claude Code MUST NOT skip or defer verification. All additions of domain enumerations and validation schemas must be audited via automated continuous integration checks and peer code reviews.
</enforcement>