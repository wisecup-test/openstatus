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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CTX-001** MUST: Developers MUST decouple asynchronous side-effect operations from incoming request cancellation by deriving execution contexts through context.WithoutCancel.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>