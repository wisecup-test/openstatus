---
version: 2
name: cross-cutting-mock-harnesses-test-fixture-generators-8fc3
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Mock Harnesses Test Fixture Generators Notification"
metadata:
  actual:
    adr_id: 8fc38acb-27a7-4489-947a-2cddb3112920
    status: accepted
    candidate_group_id: 81023cd7-8840-4276-be71-24ccf60319ea
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Mock Harnesses Test Fixture Generators Notification

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all notification integration packages consuming database entities or notification payloads, and mock utilities, test fixtures, and testing harnesses simulating notification data structures.

### Rules

- **R-DB-001** MUST: Mock harnesses and test fixture generators for notification integrations MUST consume identical entity schemas from @openstatus/db/src/schema to guarantee fixture parity with production handlers.

### Verify

```bash
# Discover workspace configuration and run static type checking across notification packages
# Run module dependency linting script to verify database schema imports originate solely from @openstatus/db/src/schema
# Execute test execution script for notification packages to verify mock harness compatibility
```

**Accept when:**
- All notification service modules and mock harnesses resolve schema imports from @openstatus/db/src/schema with zero compilation or type resolution errors.
- Static dependency boundary checks verify that no duplicate or shadowed database schema definitions exist within notification packages.
- Automated test suites for notification services and mock utilities execute successfully using the canonical schema contracts.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, static types, and module import boundaries enforce these rules.
</enforcement>