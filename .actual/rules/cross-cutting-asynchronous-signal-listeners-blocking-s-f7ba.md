---
version: 2
name: cross-cutting-asynchronous-signal-listeners-blocking-s-f7ba
description: "Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Asynchronous Signal Listeners Blocking Server Runners"
metadata:
  actual:
    adr_id: f7ba977d-6faa-4354-a470-4c47c989f4f0
    status: accepted
    candidate_group_id: c6e113eb-84af-4600-96a4-243ccd32e981
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - cmd/private/**
      - cmd/server/**
---

# Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Asynchronous Signal Listeners Blocking Server Runners

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`.

These rules are ALWAYS ACTIVE for service command entry points, long-running daemon processes managing concurrent network listeners, and background workers executing recurring updates or event processing.

### Rules

- **R-LC-001** MUST: Asynchronous signal listeners and blocking server runners MUST invoke the context cancellation function upon receiving an exit signal or encountering an unrecoverable listener error.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute concurrency linting checks.
# Discover and run the automated test suite with race detection enabled using the project execution tool.
```

**Accept when:**
- Static concurrency checks pass with zero reported goroutine leaks or unhandled context cancellations.
- Automated tests confirm that triggering context cancellation causes all background loops and listeners to exit cleanly within configured timeout bounds.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>