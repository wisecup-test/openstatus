---
version: 2
name: cross-cutting-notification-provider-modules-not-implem-63c7
description: "Adoption of @openstatus/notification-base for Notification Providers: Notification Provider Modules Not Implement Independent"
metadata:
  actual:
    adr_id: 63c7425b-c6d6-45e2-81cd-cd3355294264
    status: accepted
    candidate_group_id: 669b5cd4-f03b-4a56-83c2-fc08a7b061d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Providers: Notification Provider Modules Not Implement Independent

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for packages and modules implementing notification delivery channels, alert dispatch, and code responsible for validating and transforming incoming notification event payloads.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification provider modules MUST NOT implement independent or divergent notification core contracts outside of @openstatus/notification-base.

### Verify

```bash
# Discover and run the repository typecheck script to verify type compatibility with the base notification module across packages.
# Discover and run the workspace test runner to validate notification payload handling and dispatch flows.
# Discover and execute the repository linter script to ensure compliance with module import and boundary rules.
```

**Accept when:**
- All notification provider implementations compile cleanly against the shared base module interfaces and pass static type verification.
- Automated unit and integration test suites validating notification payload parsing and dispatch execute with zero failures.
- Payload validation schemas successfully parse valid notification inputs and reject malformed structures prior to dispatch.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>