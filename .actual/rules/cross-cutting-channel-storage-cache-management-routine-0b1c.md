---
version: 2
name: cross-cutting-channel-storage-cache-management-routine-0b1c
description: "Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Channel Storage Cache Management Routines Utilize"
metadata:
  actual:
    adr_id: 0b1cfea7-76cc-4a88-a606-f528ff43546e
    status: accepted
    candidate_group_id: f21ef219-8897-490a-8773-07efaefcb1ca
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Channel Storage Cache Management Routines Utilize

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SB-001** MUST: Channel storage and cache management routines MUST utilize slack-blocks anchor structures when persisting message delivery states and report identifiers.

### Verify

```bash
# Discover and run the workspace verification script and test suite covering notification channel formatting and anchor storage serialization.
# (Run the project's standard workspace verification and test commands as discovered from repository configuration)
```

**Accept when:**
- All message formatting routines import block layout structures exclusively from the dedicated slack-blocks internal module.
- Verification suites pass without architectural boundary violations or missing module references.
- Channel tests confirm that generated block payloads and cached anchor states match expected schema fixtures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>