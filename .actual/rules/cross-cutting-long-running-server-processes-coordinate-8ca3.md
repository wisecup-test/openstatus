---
version: 2
name: cross-cutting-long-running-server-processes-coordinate-8ca3
description: "Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Long Running Server Processes Coordinate Termination"
metadata:
  actual:
    adr_id: 8ca3bfe8-7200-4791-baa1-e7fb9c731ac1
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

# Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Long Running Server Processes Coordinate Termination

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CTX-001** MUST: Long-running server processes MUST coordinate termination through graceful shutdown procedures that allow in-flight detached tasks within their deadline windows to finish before process termination.

### Verify

```bash
echo "Discover and execute the repository test runner defined in the project build configuration"
echo "Execute static analysis and linting checks discovered from the repository manifest"
```

**Accept when:**
- Detached context routines execute to completion when parent request contexts cancel prematurely.
- All derived detached contexts enforce bounded timeouts and invoke deferred cancellation cleanly.
- Repository test suites and static analysis checks pass without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>