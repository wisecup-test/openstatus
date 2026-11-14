---
version: 2
name: cross-cutting-operations-wrapped-backoff-retry-accept-0aa4
description: "github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Operations Wrapped Backoff Retry Accept Honor"
metadata:
  actual:
    adr_id: 0aa4a96f-cd18-456f-b444-d79e6df54f93
    status: accepted
    candidate_group_id: 396c4af6-a8e5-4905-8e51-6189c2c9fb2c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - pkg/job/**
---

# github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Operations Wrapped Backoff Retry Accept Honor

Scope: applies to `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for execution of network diagnostic routines, probes, checker jobs subject to transient network failures, and any external I/O operation requiring bounded exponential backoff retry semantics.

### Rules

- **R-BACKOFF-001** MUST: Operations wrapped in backoff.Retry MUST accept and honor the cancellation signals of the passed context, terminating retry attempts immediately when the context deadline expires or cancellation is triggered.

### Verify

```bash
# Discover and execute the project's static analysis, linting scripts, and test runner across network probe packages
go test -v ./...
```

**Accept when:**
- All checker jobs utilize backoff.Retry with backoff.WithMaxTries and backoff.NewExponentialBackOff for transient network calls.
- Automated test suites pass with zero failures and confirm retry exhaustion triggers expected errors within configured timeout limits.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>