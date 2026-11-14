---
version: 2
name: cross-cutting-outbound-notification-dispatch-routines-2dcd
description: "Notification Payload Validation via slackDataSchema: Outbound Notification Dispatch Routines Not Invoke"
metadata:
  actual:
    adr_id: 2dcdb2d6-08f1-4d6d-b31a-e3d90d3016c8
    status: accepted
    candidate_group_id: ada93a1f-0699-4bec-a9b5-727b37001808
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Notification Payload Validation via slackDataSchema: Outbound Notification Dispatch Routines Not Invoke

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all notification delivery handlers that receive serialized data and invoke external client boundaries.

### Rules

- **R-NOTIF-001** MUST_NOT: Outbound notification dispatch routines MUST NOT invoke network transmission boundaries using unvalidated deserialized data structures.

### Verify

```bash
# Discover and execute the project static analysis and type checking suite to verify schema validation usage across notification dispatch boundaries.
# Discover and run automated test suites covering payload validation rejection and external client dispatch handling.
```

**Accept when:**
- All notification payload dispatch handlers pass static type checking and invoke schema validation prior to network calls.
- Automated test suites confirm that malformed payload inputs are rejected before external HTTP requests are initiated.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis and code review verification of all notification dispatch entry points. Pull requests missing schema validation prior to external dispatch must be blocked until validation is applied.
</enforcement>