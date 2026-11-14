---
version: 2
name: cross-cutting-notification-providers-validate-incoming-22f5
description: "Adopt @openstatus/notification-base for Notification Providers: Notification Providers Validate Incoming Payload Data"
metadata:
  actual:
    adr_id: 22f5288e-873a-4c4a-a408-2472c50eeb02
    status: accepted
    candidate_group_id: ab96645c-553b-4d83-8872-9ea43f28fec3
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Adopt @openstatus/notification-base for Notification Providers: Notification Providers Validate Incoming Payload Data

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Notification providers MUST validate incoming notification payload data against defined schemas before executing external delivery dispatches.

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
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing workspace compilation and test suites, as well as peer code review.
</enforcement>