---
version: 2
name: cross-cutting-components-handling-client-connections-p-328f
description: "Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Components Handling Client Connections Periodic Operations"
metadata:
  actual:
    adr_id: 328fd465-4644-4d45-a493-0d730dc30aba
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

# Context Package Goroutine Lifecycle and Signal-Driven Cancellation: Components Handling Client Connections Periodic Operations

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`.

These rules are ALWAYS ACTIVE for service command entry points, long-running daemon processes managing concurrent network listeners or polling tasks, and background workers executing recurring updates or event processing.

### Rules

- **R-LIFE-001** SHOULD: Components handling client connections and periodic operations SHOULD pass the active context instance to all network clients and synchronization primitives.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute concurrency linting checks.
# Discover and run the automated test suite with race detection enabled using the project execution tool.
```

**Accept when:**
- Static concurrency checks pass with zero reported goroutine leaks or unhandled context cancellations.
- Automated tests confirm that triggering context cancellation causes all background loops and listeners to exit cleanly within configured timeout bounds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated continuous integration pipeline running static analysis and race detection tests, and mandatory peer code review is mandatory.
</enforcement>