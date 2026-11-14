---
version: 2
name: cross-cutting-schema-composition-consumer-modules-exte-4b33
description: "Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Schema Composition Consumer Modules Extend Canonical"
metadata:
  actual:
    adr_id: 4b33e789-472c-4330-b8b1-b675b54c3624
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

# Adoption of @openstatus/db/src/schema/constants for Canonical Domain Schema Definitions: Schema Composition Consumer Modules Extend Canonical

Scope: applies to `apps/web/src/app/api/checker/cron/**`, `apps/web/src/lib/**`.

These rules are ALWAYS ACTIVE for all application modules, probe engines, API handlers, and background tasks defining or validating shared domain entities.

### Rules

- **R-SCH-001** SHOULD: Schema composition in consumer modules SHOULD extend canonical schema constants using validation library primitives to maintain type and validation parity across runtime boundaries.

### Verify

```bash
# Discover and execute the repository module boundary lint checks to ensure all domain constants are imported from the designated shared module.
# Discover and run the project type validation and test suites to verify that schema composition resolves without errors across all consumer packages.
```

**Accept when:**
- All imports of domain constants and region definitions reference the centralized schema constants module.
- Verification checks pass across all consuming modules with no duplicated domain schema constants.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and module boundary lint rules in continuous integration enforce these rules.
</enforcement>