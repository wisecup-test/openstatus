---
version: 2
name: cross-cutting-before-implementing-modifying-dependenci-6fb2
description: "Notification Payload Schema Validation via discordDataSchema: Before Implementing Modifying Dependencies Supporting Schema"
metadata:
  actual:
    adr_id: 6fb2d458-02b1-48dd-84e0-8b5cec05d132
    status: accepted
    candidate_group_id: e6559c7b-2d30-4d95-8f3d-c1a0acdb1956
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Notification Payload Schema Validation via discordDataSchema: Before Implementing Modifying Dependencies Supporting Schema

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for notification handling modules processing serialized event data for external client dispatch and integration boundaries transforming stored event data into external service payloads.

### Rules

- **R-NOTIF-001** MUST: Before implementing or modifying dependencies supporting schema definitions and notification utilities, consumers MUST inspect the repository lock artifact to determine and adhere to the exact resolved versions.
- **R-NOTIF-002** MUST: Execute discordDataSchema.parse immediately after deserializing notification data to establish a defensive perimeter protecting outbound HTTP integrations from malformed event structures.
- **R-NOTIF-003** MUST: Locate the schema definition for discordDataSchema within the repository modules and verify all required properties before invoking the parse method.
- **R-NOTIF-004** MUST: Ensure that deserialization errors and schema parse errors are distinguished in monitoring telemetry to facilitate debugging.

### Verify

```bash
# Discover the repository type check script from the project manifest and execute it
# (Replace with discovered type check command, e.g., npm run typecheck / yarn tsc)
# Discover the repository test script from the project manifest and execute the test suite
# (Replace with discovered test command, e.g., npm test / yarn test)
```

**Accept when:**
- The discovered type check script completes with zero errors across all notification delivery modules.
- Unit and integration tests validating that invalid notification payloads are rejected by schema parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All external client dispatch handlers must validate deserialized data through schema parsing.
</enforcement>