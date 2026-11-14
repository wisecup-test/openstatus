---
version: 2
name: cross-cutting-every-context-derived-via-withoutcancel-5b74
description: "Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Every Context Derived Via Withoutcancel Wrapped"
metadata:
  actual:
    adr_id: 5b74933b-6da2-4b52-82e2-eea5686e57df
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

# Context Package Asynchronous Lifecycle Isolation via context.WithoutCancel: Every Context Derived Via Withoutcancel Wrapped

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for asynchronous downstream status reporting, telemetry forwarding, and audit dispatch triggered within request handlers, as well as background operations that require access to parent context values but must survive parent cancellation.

### Rules

- **R-CTX-001** MUST: Every context derived via context.WithoutCancel MUST be wrapped in context.WithTimeout to enforce a deterministic execution boundary and prevent unbounded resource consumption.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>