---
version: 2
name: cross-cutting-consumers-discover-ecosystem-lock-artifa-f63b
description: "Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Consumers Discover Ecosystem Lock Artifact Verify"
metadata:
  actual:
    adr_id: f63b83f0-c1b9-4016-8f70-05e4fa869f3f
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

# Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Consumers Discover Ecosystem Lock Artifact Verify

Scope: applies to `apps/web/src/app/api/checker/cron/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application modules, probe engines, API handlers, and background tasks defining or validating shared domain entities.

### Rules

- **R-CAN-001** MUST: Consumers MUST discover the ecosystem lock artifact and verify the exact resolved version of internal and external workspace dependencies before implementation.

### Verify

```bash
# Discover and execute repository module boundary lint checks to ensure all domain constants are imported from the designated shared module.
# Discover and run project type validation and test suites to verify that schema composition resolves without errors across all consumer packages.
```

**Accept when:**
- All imports of domain constants and region definitions reference the centralized schema constants module.
- Verification checks pass across all consuming modules with no duplicated domain schema constants.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks reject pull requests containing duplicated domain constant declarations or violations.
</enforcement>