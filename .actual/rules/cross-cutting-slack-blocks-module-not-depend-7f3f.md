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

- **R-SB-001** SHOULD NOT: The slack-blocks module SHOULD NOT depend directly on transport clients or external database schemas, preserving its status as a modular formatting and anchor domain.
- **R-SB-002** MANDATORY: Design exported functions within the slack-blocks module to produce pure data objects without side effects or embedded transport client invocations.
- **R-SB-003** MANDATORY: Ensure anchor keys and block identifier generators encapsulate predictable schemas to facilitate safe cache lookup and deterministic message updates.
- **R-SB-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API exists, and re-run at point of use for version-sensitive behavior.

### Verify

```bash
# Discover and execute workspace verification script for type consistency and architectural boundary compliance
# Discover and execute project test suite covering notification channel formatting and anchor storage serialization
```

**Accept when:**
- All message formatting routines import block layout structures exclusively from the dedicated slack-blocks internal module.
- Verification suites pass without architectural boundary violations or missing module references.
- Channel tests confirm that generated block payloads and cached anchor states match expected schema fixtures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and module import boundary checks during pull request validation are mandatory.
</enforcement>