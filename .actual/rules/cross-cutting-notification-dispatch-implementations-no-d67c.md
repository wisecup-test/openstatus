---
version: 2
name: cross-cutting-notification-dispatch-implementations-no-d67c
description: "Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Notification Dispatch Implementations Not Redefine Locally"
metadata:
  actual:
    adr_id: d67c13fc-e1f8-4b73-bb52-78a12377a26c
    status: accepted
    candidate_group_id: 81023cd7-8840-4276-be71-24ccf60319ea
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Adoption of @openstatus/db/src/schema for Centralized Database Schema Contracts: Notification Dispatch Implementations Not Redefine Locally

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all notification integration packages consuming database entities or notification payloads, and mock utilities, test fixtures, and testing harnesses simulating notification data structures.

### Rules

- **R-DB-001** MUST_NOT: Notification dispatch implementations MUST NOT redefine or locally duplicate database schemas or entity interfaces defined in @openstatus/db/src/schema.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>