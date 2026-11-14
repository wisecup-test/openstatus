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

These rules are ALWAYS ACTIVE for validation schemas, data retrieval utilities, user interface components, and application layers integrating with scheduling intervals, regional metadata, or database model definitions.

### Rules

- **R-DOM-001** SHOULD: Presentation layers and visualization charts SHOULD consume domain status constants via @openstatus/db/src/schema/constants rather than hardcoding presentation-specific equivalents.

### Verify

```bash
# Discover and run the project static analysis and linting scripts
# Example: pnpm lint / npm run lint

# Discover and execute the type check and test suites across all workspace packages
# Example: pnpm test / npm run test
```

**Accept when:**
- Static type analysis passes across all workspace packages without type errors or resolution failures referencing the schema constants module.
- Automated test suites confirm schema validation correctly accepts canonical enumerations and rejects invalid inputs.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute workspace type verification and lint rules, and pull requests containing hardcoded duplicate domain strings or unapproved local constants must be blocked until refactored.
</enforcement>