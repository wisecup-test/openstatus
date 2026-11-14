---
version: 2
name: cross-cutting-notification-dispatch-error-handling-log-c84f
description: "Adoption of @openstatus/notification-base for Notification Providers: Notification Dispatch Error Handling Log Failure"
metadata:
  actual:
    adr_id: c84f6b5a-c579-4734-b4d3-fe1467be3404
    status: accepted
    candidate_group_id: 669b5cd4-f03b-4a56-83c2-fc08a7b061d9
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Adoption of @openstatus/notification-base for Notification Providers: Notification Dispatch Error Handling Log Failure

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for packages and modules implementing notification delivery channels and alert dispatch, and code responsible for validating and transforming incoming notification event payloads.

### Rules

- **R-NOT-001** SHOULD: Notification dispatch error handling SHOULD log failure details while preventing leakage of sensitive credentials or endpoints.

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
Claude Code MUST NOT skip or defer verification. All notification provider implementations must adhere to base module contracts and separation of dispatch boundaries.
</enforcement>