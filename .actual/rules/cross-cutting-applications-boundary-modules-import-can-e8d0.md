---
version: 2
name: cross-cutting-applications-boundary-modules-import-can-e8d0
description: "Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Applications Boundary Modules Import Canonical Domain"
metadata:
  actual:
    adr_id: e8d0478d-3c6a-4f44-a6ce-b53cf2966f54
    status: accepted
    candidate_group_id: 8d8829c5-85f5-4150-b1de-f3946448d484
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
      - src/lib/**
---

# Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Applications Boundary Modules Import Canonical Domain

Scope: applies to `apps/web/src/app/api/checker/cron/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application modules, probe engines, API handlers, and background tasks defining or validating shared domain entities.

### Rules

- **R-DOMAIN-001** MUST: Applications and boundary modules MUST import canonical domain constants, enumerations, and schema primitives from @openstatus/db/src/schema/constants rather than redefining domain constants locally.

### Verify

```bash
# Discover and execute the repository module boundary lint checks
# Discover and run the project type validation and test suites
npx turbo lint typecheck test
```

**Accept when:**
- All imports of domain constants and region definitions reference the centralized schema constants module.
- Verification checks pass across all consuming modules with no duplicated domain schema constants.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and module boundary lint rules in continuous integration reject pull requests containing duplicated domain constant declarations.
</enforcement>