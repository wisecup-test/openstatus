---
version: 2
name: cross-cutting-notification-provider-packages-adopt-ope-9cc8
description: "Adopt @openstatus/notification-base for Notification Providers: Notification Provider Packages Adopt Openstatus Base"
metadata:
  actual:
    adr_id: 9cc89ffa-8c50-4bdb-94fe-b56a900ecf05
    status: accepted
    candidate_group_id: ab96645c-553b-4d83-8872-9ea43f28fec3
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Adopt @openstatus/notification-base for Notification Providers: Notification Provider Packages Adopt Openstatus Base

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all notification provider packages and modules responsible for dispatching external notifications.

### Rules

- **R-NOTIF-001** MUST: Notification provider packages MUST adopt @openstatus/notification-base as the core foundation module for implementing notification delivery.

### Verify

```bash
# Discover and run the project workspace type-check script to verify module interface compatibility.
# Discover and run the test suite for notification provider packages to validate delivery dispatch behavior.
# Discover and run the repository linter to verify dependency boundary compliance across internal packages.
```

**Accept when:**
- The notification provider compiles with zero type errors when referencing @openstatus/notification-base.
- Payload validation succeeds for conformant data and rejects invalid inputs prior to external network requests.
- All discovered package test suites pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All notification provider implementations must use @openstatus/notification-base and pass validation checks.
</enforcement>