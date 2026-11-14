---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-0c36
description: "@openstatus/notification-base External Client Integration: Developers Inspect Repository Dependency Manifest Authoritative"
metadata:
  actual:
    adr_id: 0c364871-a70f-4bba-b063-1afa38c91653
    status: accepted
    candidate_group_id: 950fe001-2683-435a-b1e7-79e837fe1ef9
    evidence:
      source: deep
  scope:
    root: packages/subscriptions
    paths:
      - src/channels/**
---

# @openstatus/notification-base External Client Integration: Developers Inspect Repository Dependency Manifest Authoritative

Scope: applies to `packages/subscriptions/src/channels/**`.

These rules are ALWAYS ACTIVE for all outbound HTTP requests dispatched to external client endpoints within notification delivery channels, including subscription webhook delivery, verification, and test notification dispatch routines.

### Rules

- **R-NOTIF-001** MUST: Developers MUST inspect the repository dependency manifest and authoritative lock artifact to resolve and verify the exact installed library versions prior to implementing or extending outbound notification channel clients.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>