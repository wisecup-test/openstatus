---
version: 2
name: cross-cutting-status-presentation-views-notification-h-2667
description: "Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Status Presentation Views Notification Handlers Consume"
metadata:
  actual:
    adr_id: 26670f72-8c7c-47b1-adec-1c2d73a7d41b
    status: accepted
    candidate_group_id: 8a4fe0ef-90ea-4fed-b69f-f54f0e8f7fa9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/content/**
      - src/lib/**
---

# Adoption of @openstatus/api/src/router/effective-status for Centralized Effective Status Evaluation: Status Presentation Views Notification Handlers Consume

Scope: applies to `apps/web/src/app/**`, `apps/web/src/content/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EFF-001** SHOULD: Status presentation views and notification handlers SHOULD consume standardized data structures exported by @openstatus/api/src/router/effective-status to maintain cross-layer consistency.

### Verify

```bash
# Discover and execute the project dependency resolution validation script to confirm lock-artifact consistency.
# Discover and execute the repository static analysis and module boundary linter to verify import conformance with the shared router module.
# Discover and execute the automated test suite covering effective status calculation and consumer workflows.
```

**Accept when:**
- All status calculations and display surfaces derive state exclusively through the exported interfaces of the effective status router.
- Static boundary verification confirms zero direct or duplicated status evaluation implementations across consumer modules.
- The project verification suite passes without warnings or unresolved dependencies.

<enforcement>
Claude Code MUST NOT skip or defer verification. All status calculations and display surfaces must derive state through the exported interfaces of the effective status router, and static boundary verification must confirm zero direct or duplicated status evaluation implementations.
</enforcement>