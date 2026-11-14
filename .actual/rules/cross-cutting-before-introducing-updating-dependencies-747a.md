---
version: 2
name: cross-cutting-before-introducing-updating-dependencies-747a
description: "Twilio Messages API Direct Integration: Before Introducing Updating Dependencies That Integrate"
metadata:
  actual:
    adr_id: 747a774b-59e1-4831-a911-bd64dbe50596
    status: accepted
    candidate_group_id: b683c972-a3f6-40cc-bc09-fb62836ba709
    evidence:
      source: deep
  scope:
    root: packages/notifications/twillio-sms
    paths:
      - src/**
---

# Twilio Messages API Direct Integration: Before Introducing Updating Dependencies That Integrate

Scope: applies to `packages/notifications/twillio-sms/src/**`.

These rules are ALWAYS ACTIVE for outbound SMS notification dispatch modules interacting with external telephony providers.

### Rules

- **R-TWILIO-001** MUST: Before introducing or updating dependencies that integrate with external communication providers, consumers MUST inspect the repository lock artifact to determine the exact resolved dependency versions.

### Verify

```bash
# Discover and run the project static analysis and test validation suites from the repository root
# Discover and execute contract tests verifying outbound HTTP request serialization against external provider specifications
```

**Accept when:**
- All static analysis and schema validation suites complete with zero errors
- Outbound HTTP message serialization tests pass with expected authentication headers and parameter payloads

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>