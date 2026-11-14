---
version: 2
name: cross-cutting-notification-channel-provider-modules-ad-01fd
description: "Adoption of @openstatus/notification-base for Notification Providers: Notification Channel Provider Modules Adopt Import"
metadata:
  actual:
    adr_id: 01fd517d-a9dc-45b2-be32-7b4ddaced019
    status: accepted
    candidate_group_id: 669b5cd4-f03b-4a56-83c2-fc08a7b061d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Providers: Notification Channel Provider Modules Adopt Import

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for packages and modules implementing notification delivery channels, alert dispatch, and payload validation/transformation.

### Rules

- **R-NOTIF-001** MUST: Notification channel provider modules MUST adopt and import @openstatus/notification-base for shared notification contracts and interfaces.

### Verify

```bash
# Discover and run the repository typecheck script
# Discover and run the workspace test runner
# Discover and execute the repository linter script
```

**Accept when:**
- All notification provider implementations compile cleanly against the shared base module interfaces and pass static type verification.
- Automated unit and integration test suites validating notification payload parsing and dispatch execute with zero failures.
- Payload validation schemas successfully parse valid notification inputs and reject malformed structures prior to dispatch.

<enforcement>
Claude Code MUST NOT skip or defer verification. All notification provider implementations must be verified through automated continuous integration checks, peer code review, and adherence to the base module contracts.
</enforcement>