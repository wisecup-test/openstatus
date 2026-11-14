---
version: 2
name: cross-cutting-outbound-webhook-clients-not-dispatch-cbae
description: "Schema-Based Input Validation via googleChatDataSchema.parse for Notification Payloads: Outbound Webhook Clients Not Dispatch Http"
metadata:
  actual:
    adr_id: cbae54fa-63a6-493d-b7be-7bcc49f37c3a
    status: accepted
    candidate_group_id: 46c9d68e-5833-432a-8164-b85f8973d04a
    evidence:
      source: deep
  scope:
    root: packages/notifications/google-chat
    paths:
      - src/**
---

# Schema-Based Input Validation via googleChatDataSchema.parse for Notification Payloads: Outbound Webhook Clients Not Dispatch Http

Scope: applies to `packages/notifications/google-chat/src/**`.

These rules are ALWAYS ACTIVE for incoming notification payload handling routines and deserialization of external or persisted notification data prior to outbound integration requests.

### Rules

- **R-NOTIF-001** MUST_NOT: Outbound webhook clients MUST NOT dispatch HTTP requests using unvalidated notification payload data.

### Verify

```bash
# Discover the test runner from the project manifest and run automated tests targeting notification payload validation suites.
# Discover the static analysis and type verification script from the repository configuration and execute project verification.
```

**Accept when:**
- All schema validation test cases covering valid and malformed payload inputs pass without regression.
- Static type verification and lint analysis succeed across all notification packages without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>