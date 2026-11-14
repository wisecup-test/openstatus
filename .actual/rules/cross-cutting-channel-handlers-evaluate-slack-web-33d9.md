---
version: 2
name: cross-cutting-channel-handlers-evaluate-slack-web-33d9
description: "Adoption of @slack/web-api for Slack Notification Channels: Channel Handlers Evaluate Slack Web Error"
metadata:
  actual:
    adr_id: 33d9fcf5-f9f5-4f77-825d-b22dff6f2cd3
    status: accepted
    candidate_group_id: 09397f9c-705f-4f74-b2ec-9caf7cffb579
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @slack/web-api for Slack Notification Channels: Channel Handlers Evaluate Slack Web Error

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all notification delivery modules responsible for formatting and sending messages to Slack channels, and modules constructing and assembling message blocks for Slack status distributions.

### Rules

- **R-SLACK-001** MUST: Channel handlers MUST evaluate Slack Web API error responses and distinguish terminal delivery failures from transient network errors to ensure accurate subscriber state updates.

### Verify

```bash
# Discover the repository dependency management scripts and execute the static analysis suite to verify that all Slack platform interactions route exclusively through the approved client module.
# Locate and execute the test runner for the channel delivery test suite to verify that message block formatting and chat update operations handle platform responses as expected.
```

**Accept when:**
- Static analysis confirms all Slack platform communication is routed through the approved SDK client without direct HTTP requests to Slack endpoints.
- Automated test suites for channel dispatching pass with verified handling of both successful message updates and terminal API failure responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. All Slack platform interactions must route exclusively through the approved client module, and error responses must be evaluated to distinguish terminal delivery failures from transient network errors.
</enforcement>