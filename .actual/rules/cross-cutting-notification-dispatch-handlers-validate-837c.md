---
version: 2
name: cross-cutting-notification-dispatch-handlers-validate-837c
description: "Notification Payload Schema Validation via discordDataSchema: Notification Dispatch Handlers Validate Deserialized Payload"
metadata:
  actual:
    adr_id: 837cdf5f-da01-4c10-b0ff-de09a408e5bd
    status: accepted
    candidate_group_id: e6559c7b-2d30-4d95-8f3d-c1a0acdb1956
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Notification Payload Schema Validation via discordDataSchema: Notification Dispatch Handlers Validate Deserialized Payload

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for notification handling modules processing serialized event data for external client dispatch and integration boundaries transforming stored event data into external service payloads.

### Rules

- **R-NOTIF-001** MUST: Notification dispatch handlers MUST validate all deserialized payload data through discordDataSchema.parse prior to constructing external client request bodies or initiating external transmissions.

### Verify

```bash
# Discover the repository type check script from the project manifest and execute it
# Discover the repository test script from the project manifest and execute the test suite
```

**Accept when:**
- The discovered type check script completes with zero errors across all notification delivery modules.
- Unit and integration tests validating that invalid notification payloads are rejected by schema parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute repository test and validation scripts on pull requests, and peer code review confirms all external client dispatch handlers validate deserialized data through schema parsing.
</enforcement>