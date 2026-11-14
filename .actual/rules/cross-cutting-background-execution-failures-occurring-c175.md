---
version: 2
name: cross-cutting-background-execution-failures-occurring-c175
description: "Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Background Execution Failures Occurring Within Detached"
metadata:
  actual:
    adr_id: c17541c0-34bc-44bf-b231-17e29e270f5e
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

# Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Background Execution Failures Occurring Within Detached

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all asynchronous downstream status reporting, telemetry forwarding, and audit dispatch triggered within request handlers, and background operations that require access to parent context values but must survive parent cancellation.

### Rules

- **R-CTX-001** SHOULD: Background execution failures occurring within detached context routines SHOULD be recorded to structured logging destinations with descriptive contextual attributes.

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