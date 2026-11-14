---
version: 2
name: cross-cutting-long-running-background-tasks-polling-062b
description: "Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Long Running Background Tasks Polling Loops"
metadata:
  actual:
    adr_id: 062b293c-65b1-43d9-8487-cce25376edae
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

# Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Long Running Background Tasks Polling Loops

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`.

These rules are ALWAYS ACTIVE for service command entry points, long-running daemon processes, and background workers executing recurring updates or event processing.

### Rules

- **R-CTX-001** MUST: Long-running background tasks and polling loops MUST evaluate context cancellation through non-blocking or multiplexed channel selection against the context done channel to terminate execution promptly.

### Verify

```bash
# Discover and run the concurrency checks and automated test suite with race detection enabled using the project execution tool and manifest
# Example verification steps derived from repository structure:
# go test -race ./...
```

**Accept when:**
- Static concurrency checks pass with zero reported goroutine leaks or unhandled context cancellations.
- Automated tests confirm that triggering context cancellation causes all background loops and listeners to exit cleanly within configured timeout bounds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration and peer code review.
</enforcement>