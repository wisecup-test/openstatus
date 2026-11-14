---
version: 2
name: cross-cutting-notification-dispatch-handlers-not-pass-dace
description: "Notification Payload Schema Validation via discordDataSchema: Notification Dispatch Handlers Not Pass Unvalidated"
metadata:
  actual:
    adr_id: dace1759-04ff-4a77-b60c-2cc9e6b15640
    status: accepted
    candidate_group_id: e6559c7b-2d30-4d95-8f3d-c1a0acdb1956
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Notification Payload Schema Validation via discordDataSchema: Notification Dispatch Handlers Not Pass Unvalidated

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for notification handling modules processing serialized event data for external client dispatch and integration boundaries transforming stored event data into external service payloads.

### Rules

- **R-NOTIF-001** MUST_NOT: Notification dispatch handlers MUST NOT pass unvalidated deserialized data directly into external client request payloads.

### Verify

```bash
# Discover the repository type check script from the project manifest and execute it
# Discover the repository test script from the project manifest and execute it
```

**Accept when:**
- The discovered type check script completes with zero errors across all notification delivery modules.
- Unit and integration tests validating that invalid notification payloads are rejected by schema parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>