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

### Verify

```bash
# Discover the repository type check script from the project manifest and execute it
# Discover the repository test script from the project manifest and execute the test suite
```

**Accept when:**
- The discovered type check script completes with zero errors across all notification delivery modules.
- Unit and integration tests validating that invalid notification payloads are rejected by schema parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>