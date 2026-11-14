---
version: 2
name: cross-cutting-external-client-webhook-configurations-d-cb18
description: "@openstatus/notification-base External Client Integration: External Client Webhook Configurations Dynamic Payloads"
metadata:
  actual:
    adr_id: cb182bed-aea7-4feb-9b60-49150b4345c4
    status: accepted
    candidate_group_id: 950fe001-2683-435a-b1e7-79e837fe1ef9
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# @openstatus/notification-base External Client Integration: External Client Webhook Configurations Dynamic Payloads

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all outbound HTTP requests dispatched to external client endpoints within notification delivery channels.

### Rules

- **R-EXT-001** MUST: All external client webhook configurations and dynamic payloads MUST undergo structural schema validation prior to initiating the HTTP request.

### Verify

```bash
# Discover and run test suites covering notification channel client dispatches based on the project manifest
# Discover and run static analysis and type-checking scripts
# Discover and run linting verification
```

**Accept when:**
- All unit and integration test suites for notification channel clients pass with zero failures.
- Static type checking passes across all channel boundary implementations with zero errors.
- Outbound client requests verify timeout termination and signal propagation when external endpoints do not respond within configured thresholds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline failure blocks pull request merging on missing timeout signals or unvalidated schemas.
</enforcement>