---
version: 2
name: cross-cutting-frontend-modules-rendering-service-healt-3a4f
description: "Adoption of @openstatus/ui Status Block Components and Shared Types: Frontend Modules Rendering Service Health Status"
metadata:
  actual:
    adr_id: 3a4f28d6-138a-4159-aefa-374c6a84e2e2
    status: accepted
    candidate_group_id: 6ffc73a3-906b-49a6-bf40-eca1e97f23d2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# Adoption of @openstatus/ui Status Block Components and Shared Types: Frontend Modules Rendering Service Health Status

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for frontend status dashboards, incident timeline views, and service history displays rendering service health status bars and incident histories.

### Rules

- **R-OPENSTATUS-001** MUST: All frontend modules rendering service health status bars and incident histories MUST import and consume status visualization components and status data type definitions directly from @openstatus/ui.

### Verify

```bash
# Discover and execute the repository type-checking script across all workspace packages to ensure compliance with shared type contracts.
# Discover and run the repository linter to detect unapproved local status type definitions or bypassed package imports.
# Execute the automated test suite to verify correct rendering and aggregation of status timeline components.
```

**Accept when:**
- All components rendering status timelines import presentation blocks and type contracts exclusively from @openstatus/ui.
- Static type checking and automated test suites confirm zero type mismatches across status calculations and UI bindings.
- Workspace verification pipelines pass with zero unresolved dependencies or unregistered module imports.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated type checking, linting, and peer code review.
</enforcement>