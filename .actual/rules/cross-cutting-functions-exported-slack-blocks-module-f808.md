---
version: 2
name: cross-cutting-functions-exported-slack-blocks-module-f808
description: "Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Functions Exported Slack Blocks Module Maintain"
metadata:
  actual:
    adr_id: f80804dd-6c1d-4f74-8ecc-57003541c292
    status: accepted
    candidate_group_id: f21ef219-8897-490a-8773-07efaefcb1ca
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Functions Exported Slack Blocks Module Maintain

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SB-001** SHOULD: Functions exported by the slack-blocks module SHOULD maintain deterministic serializability for seamless compatibility with both transmission payloads and cache storage serialization.

### Verify

```bash
# Discover and run workspace verification script for type consistency and architectural boundary compliance
# Discover and run the project test suite covering notification channel formatting and anchor storage serialization
```

**Accept when:**
- All message formatting routines import block layout structures exclusively from the dedicated slack-blocks internal module.
- Verification suites pass without architectural boundary violations or missing module references.
- Channel tests confirm that generated block payloads and cached anchor states match expected schema fixtures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and module import boundary checks during pull request validation apply.
</enforcement>