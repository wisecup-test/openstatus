---
version: 2
name: cross-cutting-outbound-external-client-requests-not-1b70
description: "@openstatus/notification-base External Client Integration: Outbound External Client Requests Not Initiated"
metadata:
  actual:
    adr_id: 1b70af65-eb74-4717-9716-350292a5fdff
    status: accepted
    candidate_group_id: 950fe001-2683-435a-b1e7-79e837fe1ef9
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# @openstatus/notification-base External Client Integration: Outbound External Client Requests Not Initiated

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all outbound HTTP requests dispatched to external client endpoints within notification delivery channels.

### Rules

- **R-EXT-001** MUST_NOT: Outbound external client requests MUST NOT be initiated without an active timeout signal or with an unbounded connection duration.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration workflows validate static analysis, type checking, and unit test suites. Peer code reviews are mandatory for any modifications to notification channel dispatch logic.
</enforcement>