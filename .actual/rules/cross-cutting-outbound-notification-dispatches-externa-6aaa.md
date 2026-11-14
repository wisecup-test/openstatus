---
version: 2
name: cross-cutting-outbound-notification-dispatches-externa-6aaa
description: "@openstatus/notification-base External Client Integration: Outbound Notification Dispatches External Client Destinations"
metadata:
  actual:
    adr_id: 6aaae4a0-c22a-4fb1-b756-d5ceffa270ff
    status: accepted
    candidate_group_id: 950fe001-2683-435a-b1e7-79e837fe1ef9
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# @openstatus/notification-base External Client Integration: Outbound Notification Dispatches External Client Destinations

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EXT-001** MUST: Outbound notification dispatches to external client destinations managed through @openstatus/notification-base MUST be executed via HTTP POST requests bound to explicit AbortSignal timeout instances to establish a deterministic execution deadline.

### Verify

```bash
# Discover and run test runner, static analysis, type checking, and linting from project manifest
# Example discovery commands (project-dependent):
npm test
npm run typecheck
npm run lint
```

**Accept when:**
- All unit and integration test suites for notification channel clients pass with zero failures.
- Static type checking passes across all channel boundary implementations with zero errors.
- Outbound client requests verify timeout termination and signal propagation when external endpoints do not respond within configured thresholds.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>