---
version: 2
name: cross-cutting-external-transport-dispatch-logic-remain-c9c4
description: "Adoption of @openstatus/notification-base for Notification Providers: External Transport Dispatch Logic Remain Isolated"
metadata:
  actual:
    adr_id: c9c43922-e8ef-4f90-b2f7-6acefabb51f5
    status: accepted
    candidate_group_id: 669b5cd4-f03b-4a56-83c2-fc08a7b061d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Providers: External Transport Dispatch Logic Remain Isolated

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for packages and modules implementing notification delivery channels and alert dispatch, and code responsible for validating and transforming incoming notification event payloads.

### Rules

- **R-NOTIF-001** SHOULD: External transport dispatch logic remain isolated from shared notification payload modeling and contract definitions.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute repository type verification and test suites on pull requests.
</enforcement>