---
version: 2
name: cross-cutting-notification-provider-integration-packag-857c
description: "Adoption of @openstatus/notification-base for Notification Provider Integration: Notification Provider Integration Packages Adopt Internal"
metadata:
  actual:
    adr_id: 857cae46-2280-47dc-b377-a11447eb6b10
    status: accepted
    candidate_group_id: f38733e0-f502-4a66-81d9-7b70c2173a8d
    evidence:
      source: deep
  scope:
    root: packages/notifications/telegram
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Provider Integration: Notification Provider Integration Packages Adopt Internal

Scope: applies to `packages/notifications/telegram/src/**`.

These rules are ALWAYS ACTIVE for all notification provider packages and integration modules consuming inbound notification payloads within the notification workspace.

### Rules

- **R-NOTIF-001** MUST: Notification provider integration packages MUST adopt the internal @openstatus/notification-base module as their foundational contract for receiving and processing notification payloads.

### Verify

```bash
# Discover and run workspace-wide static type-checking script to confirm full contract adherence
# Discover and execute the test suite corresponding to notification delivery integrations
```

**Accept when:**
- All notification provider packages compile without type errors against the imported base contract.
- Unit and integration tests for notification dispatch pass successfully, confirming valid schema parsing and external request dispatching.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration builds fail upon type incompatibility or unvalidated notification payload handling, and pull requests bypassing the shared base module contract are blocked until refactored.
</enforcement>