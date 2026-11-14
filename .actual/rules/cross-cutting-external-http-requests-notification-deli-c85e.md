---
version: 2
name: cross-cutting-external-http-requests-notification-deli-c85e
description: "Adopt @openstatus/notification-base for Notification Providers: External Http Requests Notification Delivery Encapsulated"
metadata:
  actual:
    adr_id: c85e073c-8cdc-494b-bf8e-4ea6725ec6a8
    status: accepted
    candidate_group_id: ab96645c-553b-4d83-8872-9ea43f28fec3
    evidence:
      source: deep
  scope:
    root: packages/notifications/ntfy
    paths:
      - src/**
---

# Adopt @openstatus/notification-base for Notification Providers: External Http Requests Notification Delivery Encapsulated

Scope: applies to `packages/notifications/ntfy/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** SHOULD: External HTTP requests for notification delivery SHOULD be encapsulated within the provider module using standard request dispatching.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>