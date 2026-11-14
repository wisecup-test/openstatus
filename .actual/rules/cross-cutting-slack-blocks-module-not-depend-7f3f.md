---
version: 2
name: cross-cutting-slack-blocks-module-not-depend-7f3f
description: "Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Slack Blocks Module Not Depend Directly"
metadata:
  actual:
    adr_id: 7f3fc13a-7363-4665-94a4-f0da6718610a
    status: accepted
    candidate_group_id: f21ef219-8897-490a-8773-07efaefcb1ca
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Slack Blocks Module Not Depend Directly

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SB-001** SHOULD_NOT: The slack-blocks module SHOULD NOT depend directly on transport clients or external database schemas, preserving its status as a modular formatting and anchor domain.

### Verify

```bash
# Discover and execute the workspace verification script defined in the project configuration
# Discover and run the project test suite covering notification channel formatting and anchor storage serialization
```

**Accept when:**
- All message formatting routines import block layout structures exclusively from the dedicated slack-blocks internal module.
- Verification suites pass without architectural boundary violations or missing module references.
- Channel tests confirm that generated block payloads and cached anchor states match expected schema fixtures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code reviews verify that message layouts are not constructed ad hoc outside the designated module.
</enforcement>