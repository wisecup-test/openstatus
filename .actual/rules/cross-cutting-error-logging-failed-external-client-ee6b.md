---
version: 2
name: cross-cutting-error-logging-failed-external-client-ee6b
description: "@openstatus/notification-base External Client Integration: Error Logging Failed External Client Dispatches"
metadata:
  actual:
    adr_id: ee6bd863-4870-45e6-b9fc-52513e30f39f
    status: accepted
    candidate_group_id: 950fe001-2683-435a-b1e7-79e837fe1ef9
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# @openstatus/notification-base External Client Integration: Error Logging Failed External Client Dispatches

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all outbound HTTP requests dispatched to external client endpoints within notification delivery channels, including subscription webhook delivery, verification, and test notification dispatch routines.

### Rules

- **R-EXT-001** MUST_NOT: Error logging for failed external client dispatches MUST NOT record unredacted sensitive target credentials, secrets, or raw destination parameters.

### Verify

```bash
# Discover and execute the repository test runner covering notification channel client dispatches
# Discover and run static analysis and type-checking scripts
# Discover and run linting verification for logging and error-redaction standards
```

**Accept when:**
- All unit and integration test suites for notification channel clients pass with zero failures.
- Static type checking passes across all channel boundary implementations with zero errors.
- Outbound client requests verify timeout termination and signal propagation when external endpoints do not respond within configured thresholds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline failure blocks pull requests on missing timeout signals or unvalidated schemas.
</enforcement>