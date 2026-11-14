---
version: 2
name: cross-cutting-notification-provider-implementations-no-79c5
description: "Adopt @openstatus/notification-base for Notification Providers: Notification Provider Implementations Not Bypass Openstatus"
metadata:
  actual:
    adr_id: 79c56aca-c363-4771-9936-294aae4337c4
    status: accepted
    candidate_group_id: ab96645c-553b-4d83-8872-9ea43f28fec3
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Adopt @openstatus/notification-base for Notification Providers: Notification Provider Implementations Not Bypass Openstatus

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all notification provider packages and modules responsible for dispatching external notifications, and integration packages implementing specific notification delivery channels.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification provider implementations MUST NOT bypass @openstatus/notification-base contracts by implementing incompatible ad-hoc notification handlers.

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
Claude Code MUST NOT skip or defer verification. Compliance is verified via automated continuous integration checks executing workspace compilation and test suites, as well as peer code review.
</enforcement>