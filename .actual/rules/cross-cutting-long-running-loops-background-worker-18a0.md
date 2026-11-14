---
version: 2
name: cross-cutting-long-running-loops-background-worker-18a0
description: "Adoption of Context Module for Lifecycle and Cancellation Management: Long Running Loops Background Worker Cycles"
metadata:
  actual:
    adr_id: 18a0f340-c3ce-4a7f-9511-f6dcb08f3a56
    status: accepted
    candidate_group_id: 0d6c9b0f-e1bf-4d13-b206-45625d6c1b76
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - cmd/private/**
      - cmd/server/**
      - pkg/job/**
      - pkg/otel/**
      - pkg/scheduler/**
      - pkg/tinybird/**
      - proto/private_location/v1/**
---

# Adoption of Context Module for Lifecycle and Cancellation Management: Long Running Loops Background Worker Cycles

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CTX-001** MUST: All long-running loops, background worker cycles, and signal-handling routines MUST monitor context completion channels and terminate execution promptly upon receiving a cancellation signal.

### Verify

```bash
# Discover and execute repository static analysis and linting suite
# Discover and execute automated test suite with race detection enabled
```

**Accept when:**
- All static analysis checks pass with zero reported violations regarding context parameter order or uninvoked cancellation functions.
- Automated test suites pass with race detection enabled, validating that asynchronous workers and network requests terminate immediately upon context cancellation without leaking concurrent routines.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>