---
version: 2
name: cross-cutting-notification-delivery-handlers-not-const-4397
description: "Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Notification Delivery Handlers Not Construct Raw"
metadata:
  actual:
    adr_id: 43973915-696a-4cbc-a2ca-399a42d2671b
    status: accepted
    candidate_group_id: f21ef219-8897-490a-8773-07efaefcb1ca
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Notification Delivery Handlers Not Construct Raw

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SLK-001** MUST_NOT: Notification delivery handlers MUST NOT construct raw Slack Block Kit JSON structures or bypass the slack-blocks module when preparing channel updates.

### Verify

```bash
# Discover and run the workspace verification script to validate type consistency and architectural boundary compliance
# Discover and run the project test suite covering notification channel formatting and anchor storage serialization
```

**Accept when:**
- All message formatting routines import block layout structures exclusively from the dedicated slack-blocks internal module.
- Verification suites pass without architectural boundary violations or missing module references.
- Channel tests confirm that generated block payloads and cached anchor states match expected schema fixtures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>