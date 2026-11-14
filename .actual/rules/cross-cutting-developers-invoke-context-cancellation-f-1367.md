---
version: 2
name: cross-cutting-developers-invoke-context-cancellation-f-1367
description: "Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Invoke Context Cancellation Function Deferred"
metadata:
  actual:
    adr_id: 1367d2f5-8f21-400e-911d-0e9054433d79
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

# Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Developers Invoke Context Cancellation Function Deferred

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all asynchronous side-effect operations and background routines initiated within inbound request handlers.

### Rules

- **R-CTX-001** MUST: Developers MUST invoke the context cancellation function using deferred cleanup immediately following context.WithTimeout creation to prevent context resource leakage.

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
Verification is mandatory for all code modifying request handlers or background concurrency routines. Claude Code MUST NOT skip or defer verification.
</enforcement>