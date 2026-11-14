---
version: 2
name: cross-cutting-notification-handlers-validate-serialize-4299
description: "Schema-Based Input Validation via googleChatDataSchema.parse for Notification Payloads: Notification Handlers Validate Serialized Payload Strings"
metadata:
  actual:
    adr_id: 42990dcc-b869-4fc4-8f2f-4330e6956df0
    status: accepted
    candidate_group_id: 46c9d68e-5833-432a-8164-b85f8973d04a
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Schema-Based Input Validation via googleChatDataSchema.parse for Notification Payloads: Notification Handlers Validate Serialized Payload Strings

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NOTIF-001** MUST: Notification handlers MUST validate serialized payload strings using googleChatDataSchema.parse immediately following deserialization before dispatching outbound requests.

### Verify

```bash
# Discover the test runner from the project manifest and run automated tests targeting notification payload validation suites.
# Discover the static analysis and type verification script from the repository configuration and execute project verification.
```

**Accept when:**
- All schema validation test cases covering valid and malformed payload inputs pass without regression.
- Static type verification and lint analysis succeed across all notification packages without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>