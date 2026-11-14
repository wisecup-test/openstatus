---
version: 2
name: cross-cutting-consumer-inspect-project-dependency-mani-360d
description: "Adoption of Context Module for Lifecycle and Cancellation Management: Consumer Inspect Project Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 360d633e-69ca-44bb-a7e0-85986d73d983
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

# Adoption of Context Module for Lifecycle and Cancellation Management: Consumer Inspect Project Dependency Manifest Lock

Scope: applies to `apps/checker/checker/**`, `apps/checker/cmd/private/**`, `apps/checker/cmd/server/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/otel/**`, `apps/checker/pkg/scheduler/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CTX-001** MUST: The consumer MUST inspect the project dependency manifest and lock artifact to verify the active runtime environment and standard library compatibility before implementing or modifying context-managed components.
- **R-CTX-002** MUST: Derive child contexts with explicit deadlines or timeouts at the initiation of network operations and immediately schedule deferred cancellation.
- **R-CTX-003** MUST: Ensure long-running scheduler tickers and message ingestion loops select on context completion channels to guarantee prompt response to shutdown signals.

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