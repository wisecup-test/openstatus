---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-4e3d
description: "Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Consumers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: 4e3dbf8f-798e-48d5-b8db-c6c6be55f0b3
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

# Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Consumers Inspect Repository Lock Artifact Verify

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`.

These rules are ALWAYS ACTIVE for service command entry points, long-running daemon processes managing concurrent network listeners or polling tasks, and background workers executing recurring updates or event processing.

### Rules

- **R-COR-001** MUST: Consumers MUST inspect the repository lock artifact to verify the exact resolved version of all runtime dependencies prior to implementing or extending concurrency coordination routines.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute concurrency linting checks.
# Discover and run the automated test suite with race detection enabled using the project execution tool.
```

**Accept when:**
- Static concurrency checks pass with zero reported goroutine leaks or unhandled context cancellations.
- Automated tests confirm that triggering context cancellation causes all background loops and listeners to exit cleanly within configured timeout bounds.

<enforcement>
Claude Code MUST NOT skip or defer verification. All modifications to service entry points and background worker loops are verified by automated continuous integration pipeline running static analysis and race detection tests, as well as mandatory peer code review.
</enforcement>