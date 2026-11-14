---
version: 2
name: cross-cutting-outbound-channel-dispatches-propagate-ca-d8cb
description: "@openstatus/notification-base External Client Integration: Outbound Channel Dispatches Propagate Cancellation Signals"
metadata:
  actual:
    adr_id: d8cb286a-e9c3-4455-b685-481ccde48882
    status: accepted
    candidate_group_id: 950fe001-2683-435a-b1e7-79e837fe1ef9
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# @openstatus/notification-base External Client Integration: Outbound Channel Dispatches Propagate Cancellation Signals

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all outbound HTTP requests dispatched to external client endpoints within notification delivery channels, including subscription webhook delivery, verification, and test notification dispatch routines.

### Rules

- **R-EXT-001** SHOULD: Outbound channel dispatches propagate cancellation signals through retry and resilience execution pipelines to coordinate backoff and cleanup on abort.
- **R-EXT-002** MANDATORY: The consumer MUST discover dependency manifests, build tools, exact resolved versions from repository locks/resolution artifacts, and fetch official documentation for version-sensitive behaviors prior to writing code that uses a versioned library.

### Verify

```bash
# Discover the repository test runner from the project manifest and execute the test suite covering notification channel client dispatches.
# Discover and run the repository static analysis and type-checking scripts to verify type safety across external client boundaries.
# Discover and run the repository linting verification to confirm compliance with logging and error-redaction standards.
```

**Accept when:**
- All unit and integration test suites for notification channel clients pass with zero failures.
- Static type checking passes across all channel boundary implementations with zero errors.
- Outbound client requests verify timeout termination and signal propagation when external endpoints do not respond within configured thresholds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration workflows, mandatory peer reviews, and strict CI pipeline rejections enforce compliance with timeout signals, schema validations, and credential redaction standards.
</enforcement>