---
version: 2
name: cross-cutting-slack-message-layouts-block-anchor-c908
description: "Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Slack Message Layouts Block Anchor Representations"
metadata:
  actual:
    adr_id: c90870bc-7bff-4d3c-898a-d2ee18d3667d
    status: accepted
    candidate_group_id: f21ef219-8897-490a-8773-07efaefcb1ca
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# Adoption of slack-blocks Internal Module for Slack Notification Payload Structuring: Slack Message Layouts Block Anchor Representations

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SLACK-001** MUST: All Slack message layouts, block anchor representations, and interactive message payloads MUST be composed and structured through the dedicated slack-blocks internal module.
- **R-SLACK-002** MANDATORY: The consumer MUST derive tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-SLACK-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding sequence (find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm API existence, re-run per dependency at point of use).

### Verify

```bash
# Discover and run the workspace verification script defined in the project configuration
# Discover and run the project test suite covering notification channel formatting and anchor storage serialization
```

**Accept when:**
- All message formatting routines import block layout structures exclusively from the dedicated slack-blocks internal module.
- Verification suites pass without architectural boundary violations or missing module references.
- Channel tests confirm that generated block payloads and cached anchor states match expected schema fixtures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, module import boundary checks, and peer code reviews enforce these requirements; unauthorized inline Slack Block Kit payload definitions or module couplings are blocked from merging.
</enforcement>