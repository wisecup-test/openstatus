---
version: 2
name: cross-cutting-provider-modules-not-duplicate-common-c98f
description: "Adoption of @openstatus/notification-base for Notification Modules: Provider Modules Not Duplicate Common Notification"
metadata:
  actual:
    adr_id: c98fd35d-5978-4829-913a-f9c602e904a3
    status: accepted
    candidate_group_id: c9667bbc-a93e-4be6-8512-d4d9f3810ba7
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Modules: Provider Modules Not Duplicate Common Notification

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for all notification integration modules and dispatchers within the notification subsystem.

### Rules

- **R-ADR-001** MUST_NOT: Provider modules MUST NOT duplicate common notification payload contracts or base schema declarations outside @openstatus/notification-base.

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