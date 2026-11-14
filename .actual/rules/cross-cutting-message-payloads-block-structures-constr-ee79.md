---
version: 2
name: cross-cutting-message-payloads-block-structures-constr-ee79
description: "Adoption of @slack/web-api for Slack Notification Channels: Message Payloads Block Structures Constructed Typed"
metadata:
  actual:
    adr_id: ee79fdde-020d-4cae-a69e-9764240a5cc7
    status: accepted
    candidate_group_id: 09397f9c-705f-4f74-b2ec-9caf7cffb579
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @slack/web-api for Slack Notification Channels: Message Payloads Block Structures Constructed Typed

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for notification delivery modules and modules constructing and assembling message blocks for Slack status distributions.

### Rules

- **R-SLACK-001** MUST: Message payloads and block structures MUST be constructed using typed representations compatible with `@slack/web-api` specifications.

### Verify

```bash
# Discover repository dependency management scripts and run static analysis
# Locate and execute the test runner for the channel delivery test suite
```

**Accept when:**
- Static analysis confirms all Slack platform communication is routed through the approved SDK client without direct HTTP requests to Slack endpoints.
- Automated test suites for channel dispatching pass with verified handling of both successful message updates and terminal API failure responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>