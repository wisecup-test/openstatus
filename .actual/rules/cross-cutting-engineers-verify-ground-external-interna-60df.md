---
version: 2
name: cross-cutting-engineers-verify-ground-external-interna-60df
description: "Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Engineers Verify Ground External Internal Dependency"
metadata:
  actual:
    adr_id: 60df6b15-9d94-49fd-a4ae-3049fe5ac8c1
    status: accepted
    candidate_group_id: f21ef219-8897-490a-8773-07efaefcb1ca
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Engineers Verify Ground External Internal Dependency

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for constructing Slack message payloads, block layouts for subscription notifications, and persisting/resolving Slack message anchor structures in channel cache layers.

### Rules

- **R-SLACK-001** MUST: Engineers MUST verify and ground all external and internal dependency versions against the project lock resolution artifact prior to implementing or extending message block interfaces.

### Verify

```bash
# Discover and execute workspace verification script and test suite
# (Commands to be discovered from project repository configuration)
```

**Accept when:**
- All message formatting routines import block layout structures exclusively from the dedicated slack-blocks internal module.
- Verification suites pass without architectural boundary violations or missing module references.
- Channel tests confirm that generated block payloads and cached anchor states match expected schema fixtures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code review enforce that pull requests containing inline Slack Block Kit payload definitions or unauthorized module couplings are blocked from merging.
</enforcement>