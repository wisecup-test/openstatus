---
version: 2
name: cross-cutting-system-use-slack-web-exclusive-531f
description: "Adoption of @slack/web-api for Slack Notification Channels: System Use Slack Web Exclusive Sdk"
metadata:
  actual:
    adr_id: 531f5d0f-9801-48b3-8ac7-a9554b2c0873
    status: accepted
    candidate_group_id: 09397f9c-705f-4f74-b2ec-9caf7cffb579
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of @slack/web-api for Slack Notification Channels: System Use Slack Web Exclusive Sdk

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SLACK-001** MUST: The system MUST use `@slack/web-api` as the exclusive SDK client for all programmatic interactions with the Slack platform across notification delivery channels.

### Verify

```bash
# Discover repository dependency management scripts and execute static analysis suite
# Locate and execute the test runner for the channel delivery test suite
```

**Accept when:**
- Static analysis confirms all Slack platform communication is routed through the approved SDK client without direct HTTP requests to Slack endpoints.
- Automated test suites for channel dispatching pass with verified handling of both successful message updates and terminal API failure responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated build gates enforce dependency import boundaries and prohibit raw HTTP calls to external vendor endpoints.
</enforcement>