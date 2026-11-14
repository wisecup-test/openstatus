---
version: 2
name: cross-cutting-any-structural-modification-database-mod-2cd6
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Any Structural Modification Database Models Consumed"
metadata:
  actual:
    adr_id: 2cd69fa8-c9d4-4d10-8a29-1fe64b7f950c
    status: accepted
    candidate_group_id: 81023cd7-8840-4276-be71-24ccf60319ea
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Any Structural Modification Database Models Consumed

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** SHOULD: Any structural modification to database models consumed by notification services SHOULD be introduced and exported within @openstatus/db/src/schema before updating consuming modules.
- **R-DB-002** MANDATORY: When introducing new notification entity structures, export the corresponding schemas from @openstatus/db/src/schema prior to consumption in notification integration modules.
- **R-DB-003** MANDATORY: Ensure mock utilities and test fixture generators reference exported schema definitions from @openstatus/db/src/schema to preserve contract parity with runtime handlers.

### Verify

```bash
# Discover the workspace configuration to locate validation scripts and run the static type checker across notification packages.
# Discover and execute the module dependency linting script to verify that database schema imports originate solely from @openstatus/db/src/schema.
# Execute the discovered test execution script for notification packages to verify mock harness compatibility against schema definitions.
```

**Accept when:**
- All notification service modules and mock harnesses resolve schema imports from @openstatus/db/src/schema with zero compilation or type resolution errors.
- Static dependency boundary checks verify that no duplicate or shadowed database schema definitions exist within notification packages.
- Automated test suites for notification services and mock utilities execute successfully using the canonical schema contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. All database entity structures must be imported from @openstatus/db/src/schema rather than declared locally.
</enforcement>