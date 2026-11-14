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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-VAL-001** MUST: Before implementing validation logic that consumes workspace or external schema packages, the consumer MUST inspect the workspace dependency manifests and resolution lock artifacts to determine the exact resolved dependency versions.
- **R-VAL-002** MUST: Ensure schema parsing errors are captured and handled with structured diagnostic logging without exposing sensitive data.
- **R-VAL-003** MUST: Align schema definitions with shared database and notification base schema packages across workspace boundaries.

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