---
version: 2
name: cross-cutting-modules-not-duplicate-hardcode-domain-7d8f
description: "Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Modules Not Duplicate Hardcode Domain Enumeration"
metadata:
  actual:
    adr_id: 7d8f449a-d454-4d0b-963d-4cf9e67ba811
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

# Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Modules Not Duplicate Hardcode Domain Enumeration

Scope: applies to `apps/web/src/app/api/checker/cron/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application modules, probe engines, API handlers, and background tasks defining or validating shared domain entities.

### Rules

- **R-DOM-001** MUST_NOT: Modules MUST NOT duplicate or hardcode domain enumeration values or identifier schemas outside the centralized schema constants definition.

### Verify

```bash
# Discover and execute the repository module boundary lint checks to ensure all domain constants are imported from the designated shared module.
# Discover and run the project type validation and test suites to verify that schema composition resolves without errors across all consumer packages.
```

**Accept when:**
- All imports of domain constants and region definitions reference the centralized schema constants module.
- Verification checks pass across all consuming modules with no duplicated domain schema constants.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks reject pull requests containing duplicated domain constant declarations.
</enforcement>