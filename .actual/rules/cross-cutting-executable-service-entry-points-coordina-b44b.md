---
version: 2
name: cross-cutting-executable-service-entry-points-coordina-b44b
description: "Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Executable Service Entry Points Coordinate Process"
metadata:
  actual:
    adr_id: b44ba4f5-0022-41e3-a4d6-06e88581feb4
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

# Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Executable Service Entry Points Coordinate Process

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`.

These rules are ALWAYS ACTIVE for service command entry points, long-running daemon processes, and background workers executing recurring updates or event processing.

### Rules

- **R-LFC-001** MUST: All executable service entry points MUST coordinate process lifecycle through the context package by creating a cancellable root context that governs downstream servers and background routines.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute concurrency linting checks.
# Discover and run the automated test suite with race detection enabled using the project execution tool.
```

**Accept when:**
- Static concurrency checks pass with zero reported goroutine leaks or unhandled context cancellations.
- Automated tests confirm that triggering context cancellation causes all background loops and listeners to exit cleanly within configured timeout bounds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration and mandatory peer code review enforce these rules.
</enforcement>