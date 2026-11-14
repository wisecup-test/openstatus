---
version: 2
name: cross-cutting-external-network-client-invocations-resi-162c
description: "Adoption of Context Module for Lifecycle and Cancellation Management: External Network Client Invocations Resilience Retry"
metadata:
  actual:
    adr_id: 162c1487-6491-42ac-871b-816be7c755b5
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

# Adoption of Context Module for Lifecycle and Cancellation Management: External Network Client Invocations Resilience Retry

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CTX-001** MUST: External network client invocations and resilience retry operations MUST bind their execution lifetime to the provided context instance to enforce operational timeouts and cooperative cancellation.

### Verify

```bash
# Discover and execute the repository static analysis and linting suite to verify context parameter ordering and deferred cancellation compliance.
# Discover and execute the automated test suite with race detection enabled to confirm cooperative cancellation and goroutine termination under timeout conditions.
```

**Accept when:**
- All static analysis checks pass with zero reported violations regarding context parameter order or uninvoked cancellation functions.
- Automated test suites pass with race detection enabled, validating that asynchronous workers and network requests terminate immediately upon context cancellation without leaking concurrent routines.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis, linting, and test suites with race detection enabled.
</enforcement>