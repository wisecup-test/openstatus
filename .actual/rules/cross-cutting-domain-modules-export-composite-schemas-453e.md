---
version: 2
name: cross-cutting-domain-modules-export-composite-schemas-453e
description: "Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Domain Modules Export Composite Schemas That"
metadata:
  actual:
    adr_id: 453e295a-5fc4-4635-87e7-3e2773a25e6b
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

# Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Domain Modules Export Composite Schemas That

Scope: applies to `apps/web/src/app/api/checker/cron/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application modules, probe engines, API handlers, and background tasks defining or validating shared domain entities.

### Rules

- **R-CANON-001** MAY: Domain modules MAY re-export composite schemas that embed constants from the shared schema constants module for subsystem-specific validation workflows.

### Verify

```bash
# Discover and execute the repository module boundary lint checks to ensure all domain constants are imported from the designated shared module.
# Discover and run the project type validation and test suites to verify that schema composition resolves without errors across all consumer packages.
```

**Accept when:**
- All imports of domain constants and region definitions reference the centralized schema constants module.
- Verification checks pass across all consuming modules with no duplicated domain schema constants.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis and module boundary lint rules in continuous integration, and peer code review.
</enforcement>