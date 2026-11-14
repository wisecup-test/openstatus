---
version: 2
name: cross-cutting-developers-decouple-asynchronous-side-ef-c546
description: "Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Decouple Asynchronous Side Effect Operations"
metadata:
  actual:
    adr_id: c5466d94-847d-4cfd-bbb3-95f59b9f4dfe
    status: accepted
    candidate_group_id: 1700c304-66aa-4383-b313-bb2df6cb6fc3
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - cmd/server/**
      - internal/server/**
---

# Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Decouple Asynchronous Side Effect Operations

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all asynchronous side-effect operations, telemetry forwarding, audit dispatch, and background tasks initiated within request handlers.

### Rules

- **R-CTX-001** MUST: Developers MUST decouple asynchronous side-effect operations from incoming request cancellation by deriving execution contexts through `context.WithoutCancel`.
- **R-CTX-002** MUST: Constrain detached contexts with `context.WithTimeout` to prevent orphan background tasks from consuming system resources indefinitely.
- **R-CTX-003** MUST: Ensure all detached contexts immediately invoke deferred cancellation functions to prevent context leaks.
- **R-CTX-004** MUST: Ensure all detached goroutines handle errors locally and record failures to structured logging.
- **R-CTX-005** MUST: Coordinate server shutdown handlers to allow active bounded detached operations sufficient window to complete.

### Verify

```bash
# Discover and execute the repository test runner defined in the project build configuration
echo "Discover and execute the repository test runner defined in the project build configuration"
# Execute static analysis and linting checks discovered from the repository manifest
echo "Execute static analysis and linting checks discovered from the repository manifest"
```

**Accept when:**
- Detached context routines execute to completion when parent request contexts cancel prematurely.
- All derived detached contexts enforce bounded timeouts and invoke deferred cancellation cleanly.
- Repository test suites and static analysis checks pass without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and peer code review enforce timeout constraints, deferred cancellation, and proper error handling on detached contexts.
</enforcement>