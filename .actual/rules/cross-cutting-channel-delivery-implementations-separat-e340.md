---
version: 2
name: cross-cutting-channel-delivery-implementations-separat-e340
description: "Adoption of @slack/web-api for Slack Notification Channels: Channel Delivery Implementations Separate Message Block"
metadata:
  actual:
    adr_id: e3406f67-722a-49d7-b1fd-015081430254
    status: accepted
    candidate_group_id: 09397f9c-705f-4f74-b2ec-9caf7cffb579
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @slack/web-api for Slack Notification Channels: Channel Delivery Implementations Separate Message Block

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for notification delivery modules responsible for formatting and sending messages to Slack channels, and modules constructing and assembling message blocks for Slack status distributions.

### Rules

- **R-SLACK-001** SHOULD: Channel delivery implementations SHOULD separate message block composition from channel delivery orchestration and subscription state updates.

### Verify

```bash
# Discover the repository dependency management scripts and execute the static analysis suite to verify that all Slack platform interactions route exclusively through the approved client module.
# Locate and execute the test runner for the channel delivery test suite to verify that message block formatting and chat update operations handle platform responses as expected.
```

**Accept when:**
- Static analysis confirms all Slack platform communication is routed through the approved SDK client without direct HTTP requests to Slack endpoints.
- Automated test suites for channel dispatching pass with verified handling of both successful message updates and terminal API failure responses.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>