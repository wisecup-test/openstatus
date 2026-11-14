---
version: 2
name: cross-cutting-notification-channel-implementations-not-98c3
description: "Adoption of @slack/web-api for Slack Notification Channels: Notification Channel Implementations Not Perform Manual"
metadata:
  actual:
    adr_id: 98c3ccbb-6adc-45e6-9609-7fa8150f5fd6
    status: accepted
    candidate_group_id: 09397f9c-705f-4f74-b2ec-9caf7cffb579
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @slack/web-api for Slack Notification Channels: Notification Channel Implementations Not Perform Manual

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for notification delivery modules and modules constructing and assembling message blocks for Slack status distributions.

### Rules

- **R-SLACK-001** MUST_NOT: Notification channel implementations MUST NOT perform manual HTTP requests to Slack Web API endpoints when equivalent client methods exist within `@slack/web-api`.

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