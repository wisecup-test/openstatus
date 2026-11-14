---
version: 2
name: cross-cutting-developers-not-dispatch-detached-backgro-e9dd
description: "Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Not Dispatch Detached Background Operations"
metadata:
  actual:
    adr_id: e9dd6204-089c-464b-bc3f-134eafe9e5b1
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

# Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Not Dispatch Detached Background Operations

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for asynchronous downstream status reporting, telemetry forwarding, and audit dispatch triggered within request handlers.

### Rules

- **R-CTX-001** MUST_NOT: Developers MUST_NOT dispatch detached background operations with unconstrained context lifetimes or pass raw detached contexts without an explicit deadline.

### Verify

```bash
# Discover and execute the repository test runner defined in the project build configuration
# Execute static analysis and linting checks discovered from the repository manifest
```

**Accept when:**
- Detached context routines execute to completion when parent request contexts cancel prematurely.
- All derived detached contexts enforce bounded timeouts and invoke deferred cancellation cleanly.
- Repository test suites and static analysis checks pass without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks in the continuous integration pipeline and peer code review of server request handlers and concurrency routines.
</enforcement>