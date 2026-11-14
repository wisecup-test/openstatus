---
version: 2
name: cross-cutting-schema-parsing-errors-captured-recorded-daad
description: "Notification Payload Schema Validation via discordDataSchema: Schema Parsing Errors Captured Recorded Contextual"
metadata:
  actual:
    adr_id: daad05aa-7254-4f86-acc7-7a5b1dd42f0f
    status: accepted
    candidate_group_id: e6559c7b-2d30-4d95-8f3d-c1a0acdb1956
    evidence:
      source: deep
  scope:
    root: packages/notifications/discord
    paths:
      - src/**
---

# Notification Payload Schema Validation via discordDataSchema: Schema Parsing Errors Captured Recorded Contextual

Scope: applies to `packages/notifications/discord/src/**`.

These rules are ALWAYS ACTIVE for notification handling modules processing serialized event data for external client dispatch and integration boundaries transforming stored event data into external service payloads.

### Rules

- **R-NOT-001** SHOULD: Schema parsing errors SHOULD be captured and recorded with contextual event identifiers while omitting sensitive payload attributes.
- **R-NOT-002** MANDATORY: Locate the schema definition for discordDataSchema within the repository modules and verify all required properties before invoking the parse method.
- **R-NOT-003** MANDATORY: Ensure that deserialization errors and schema parse errors are distinguished in monitoring telemetry to facilitate debugging.
- **R-NOT-004** MANDATORY: Before writing code that uses a versioned library, follow the lock-version grounding process to determine the exact resolved version, fetch official documentation, and confirm all APIs exist.

### Verify

```bash
# Discover and execute the repository type check script
# Discover and execute the repository test suite covering notification validation routines
```

**Accept when:**
- The discovered type check script completes with zero errors across all notification delivery modules.
- Unit and integration tests validating that invalid notification payloads are rejected by schema parsing pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute repository test and validation scripts on pull requests, and peer code review confirms all external client dispatch handlers validate deserialized data through schema parsing.
</enforcement>