---
version: 2
name: cross-cutting-notification-provider-modules-import-imp-db3a
description: "Adoption of @openstatus/notification-base for Notification Modules: Notification Provider Modules Import Implement Foundational"
metadata:
  actual:
    adr_id: db3ae009-e714-4bdc-8cba-58bbf309a605
    status: accepted
    candidate_group_id: c9667bbc-a93e-4be6-8512-d4d9f3810ba7
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Modules: Notification Provider Modules Import Implement Foundational

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Notification provider modules MUST import and implement foundational notification structures and interfaces from @openstatus/notification-base.

### Verify

```bash
# Discover and execute repository dependency boundary and module linting scripts.
# Discover and run test suites targeting notification provider modules.
```

**Accept when:**
- Static analysis confirms notification integration modules import shared notification primitives from @openstatus/notification-base.
- All automated test suites and contract validations for notification delivery pass without structural dependency violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>