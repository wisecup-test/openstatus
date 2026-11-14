---
version: 2
name: cross-cutting-validation-failures-encountered-during-s-de92
description: "Notification Payload Schema Validation via discordDataSchema: Validation Failures Encountered During Schema Parsing"
metadata:
  actual:
    adr_id: de92b6fe-c374-47da-b3e2-0eef2c411339
    status: accepted
    candidate_group_id: e6559c7b-2d30-4d95-8f3d-c1a0acdb1956
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Notification Payload Schema Validation via discordDataSchema: Validation Failures Encountered During Schema Parsing

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for notification handling modules processing serialized event data for external client dispatch and integration boundaries transforming stored event data into external service payloads.

### Rules

- **R-NOTIF-001** MUST: Validation failures encountered during schema parsing MUST fail fast and prevent outgoing external client network invocations.

### Verify

\`\`\`bash
# Discover the repository type check script from the project manifest and execute it to verify all payload references satisfy schema parsing constraints.
# Discover the repository test script from the project manifest and execute the test suite covering notification validation routines.
\`\`\`

**Accept when:**
- The discovered type check script completes with zero errors across all notification delivery modules.
- Unit and integration tests validating that invalid notification payloads are rejected by schema parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing unvalidated payload dispatch are blocked from merging until schema parsing is introduced.
</enforcement>