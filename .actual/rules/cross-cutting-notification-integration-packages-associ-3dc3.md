---
version: 2
name: cross-cutting-notification-integration-packages-associ-3dc3
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Notification Integration Packages Associated Test Mock"
metadata:
  actual:
    adr_id: 3dc39138-45a9-45b7-8b11-ce4d1046a2bd
    status: accepted
    candidate_group_id: 81023cd7-8840-4276-be71-24ccf60319ea
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Notification Integration Packages Associated Test Mock

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all notification integration packages, mock utilities, test fixtures, and testing harnesses simulating notification data structures.

### Rules

- **R-DB-001** MUST: Notification integration packages and associated test mock harnesses MUST import database schema models and entity contracts exclusively from @openstatus/db/src/schema.
- **R-DB-002** MUST: When introducing new notification entity structures, export the corresponding schemas from @openstatus/db/src/schema prior to consumption in notification integration modules.
- **R-DB-003** MUST: Ensure mock utilities and test fixture generators reference exported schema definitions from @openstatus/db/src/schema to preserve contract parity with runtime handlers.

### Verify

```bash
# Discover workspace configuration to locate validation scripts and run static type checks across notification packages
# Discover and execute module dependency linting script to verify database schema imports originate solely from @openstatus/db/src/schema
# Execute test execution script for notification packages to verify mock harness compatibility against schema definitions
```

**Accept when:**
- All notification service modules and mock harnesses resolve schema imports from @openstatus/db/src/schema with zero compilation or type resolution errors.
- Static dependency boundary checks verify that no duplicate or shadowed database schema definitions exist within notification packages.
- Automated test suites for notification services and mock utilities execute successfully using the canonical schema contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines verify static types and module import boundaries. Build and verification scripts reject modules declaring local duplicates of database schema structures.
</enforcement>