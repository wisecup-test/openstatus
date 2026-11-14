---
version: 2
name: cross-cutting-context-instances-not-stored-within-6577
description: "Adoption of Context Module for Lifecycle and Cancellation Management: Context Instances Not Stored Within Long"
metadata:
  actual:
    adr_id: 6577699c-4875-4d9e-a461-4c638fea2793
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

# Adoption of Context Module for Lifecycle and Cancellation Management: Context Instances Not Stored Within Long

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CTX-001** SHOULD: Context instances SHOULD NOT be stored within long-lived struct fields and SHOULD be passed explicitly through function call stacks.

### Verify

```bash
# Discover and execute the repository static analysis and linting suite to verify context parameter ordering and deferred cancellation compliance.
# Discover and execute the automated test suite with race detection enabled to confirm cooperative cancellation and goroutine termination under timeout conditions.
```

**Accept when:**
- All static analysis checks pass with zero reported violations regarding context parameter order or uninvoked cancellation functions.
- Automated test suites pass with race detection enabled, validating that asynchronous workers and network requests terminate immediately upon context cancellation without leaking concurrent routines.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>