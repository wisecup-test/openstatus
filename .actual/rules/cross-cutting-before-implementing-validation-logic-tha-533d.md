---
version: 2
name: cross-cutting-before-implementing-validation-logic-tha-533d
description: "Notification Payload Validation via slackDataSchema: Before Implementing Validation Logic That Consumes"
metadata:
  actual:
    adr_id: 533d3872-5b46-442e-a2b9-01a61b16639e
    status: accepted
    candidate_group_id: ada93a1f-0699-4bec-a9b5-727b37001808
    evidence:
      source: deep
  scope:
    root: packages/notifications/slack
    paths:
      - src/**
---

# Notification Payload Validation via slackDataSchema: Before Implementing Validation Logic That Consumes

Scope: applies to `packages/notifications/slack/src/**`.

These rules are ALWAYS ACTIVE for all notification delivery handlers that receive serialized data and invoke external client boundaries.

### Rules

- **R-VAL-001** MUST: Before implementing validation logic that consumes workspace or external schema packages, the consumer MUST inspect the workspace dependency manifests and resolution lock artifacts to determine the exact resolved dependency versions.
- **R-VAL-002** MUST: Execute schema validation immediately following JSON deserialization to establish an explicit validation boundary before downstream message formatting.
- **R-VAL-003** MUST: Capture schema parsing errors and handle them with structured diagnostic logging without exposing sensitive data.

### Verify

```bash
# Discover and execute the project static analysis and type checking suite to verify schema validation usage
# Discover and run automated test suites covering payload validation rejection and external client dispatch handling
```

**Accept when:**
- All notification payload dispatch handlers pass static type checking and invoke schema validation prior to network calls.
- Automated test suites confirm that malformed payload inputs are rejected before external HTTP requests are initiated.

<enforcement>
Claude Code MUST NOT skip or defer verification. All notification dispatch entry points must be verified via static analysis and code review.
</enforcement>