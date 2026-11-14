---
version: 2
name: cross-cutting-notification-dispatch-handlers-validate-98be
description: "Notification Payload Validation via slackDataSchema: Notification Dispatch Handlers Validate Deserialized Payload"
metadata:
  actual:
    adr_id: 98be8ba9-f1dc-4457-870e-037cfdd8b1c3
    status: accepted
    candidate_group_id: ada93a1f-0699-4bec-a9b5-727b37001808
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Notification Payload Validation via slackDataSchema: Notification Dispatch Handlers Validate Deserialized Payload

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all notification delivery handlers that receive serialized data and invoke external client boundaries.

### Rules

- **R-NOTIF-001** MUST: Notification dispatch handlers MUST validate deserialized notification payload data through slackDataSchema.parse prior to constructing or dispatching outbound client payloads.

### Verify

```bash
# Discover and execute the project static analysis and type checking suite
# Discover and run automated test suites covering payload validation rejection and external client dispatch handling
```

**Accept when:**
- All notification payload dispatch handlers pass static type checking and invoke schema validation prior to network calls.
- Automated test suites confirm that malformed payload inputs are rejected before external HTTP requests are initiated.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis and code review verification of all notification dispatch entry points.
</enforcement>