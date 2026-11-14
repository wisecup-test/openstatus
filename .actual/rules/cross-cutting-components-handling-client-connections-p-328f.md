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

- **R-CTX-001** SHOULD: Components handling client connections and periodic operations SHOULD pass the active context instance to all network clients and synchronization primitives.
- **R-CTX-002** MANDATORY: The consumer MUST derive tool names, file names, commands, package managers, and version numbers from the project repository according to the discovery policy.
- **R-CTX-003** MANDATORY: Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact for exact version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use.
- **R-CTX-004** MANDATORY: Construct root contexts with cancellation in the entrypoint function and pass them down into server structures and background loop routines.
- **R-CTX-005** MANDATORY: Ensure that error paths from server listeners and signal channel receivers execute cancellation callbacks exactly once.

### Verify

```bash
# Discover and execute the project verification script from the repository manifest for concurrency linting checks:
# (Discover build/test tool from repository manifest, e.g., go test -race ./...)
# Run automated test suite with race detection enabled:
$(npm run test:race || go test -race ./... || cargo test -- --test-threads=1)
```

**Accept when:**
- Static concurrency checks pass with zero reported goroutine leaks or unhandled context cancellations.
- Automated tests confirm that triggering context cancellation causes all background loops and listeners to exit cleanly within configured timeout bounds.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration pipeline and mandatory peer code reviews.
</enforcement>