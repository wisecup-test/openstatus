---
version: 2
name: cross-cutting-external-client-timeout-configurations-e-c375
description: "Resilience Retry Orchestration via github.com/cenkalti/backoff: External Client Timeout Configurations Exceed Individual"
metadata:
  actual:
    adr_id: c37537ff-f8bb-4893-b070-7038859e32eb
    status: accepted
    candidate_group_id: 1a9fc28f-a52f-4b72-9081-afceafbc050c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - handlers/**
      - pkg/job/**
---

# Resilience Retry Orchestration via github.com/cenkalti/backoff: External Client Timeout Configurations Exceed Individual

Scope: applies to `apps/checker/handlers/**`, `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RES-001** MUST: External client timeout configurations MUST exceed individual attempt thresholds but remain strictly bounded within the aggregate deadline enforced across all retry intervals.

### Verify

```bash
# Discover and run dependency audit and test suite commands from repository configuration
# Example discovery pattern:
# 1. Discover the project dependency audit script from repository configuration and execute it to verify lock-file consistency for third-party resilience libraries.
# 2. Discover and run the project test suite to ensure all network retry policies adhere to configured exponential backoff bounds and context cancellations.
# 3. Execute static analysis rules discovered from repository configuration to detect direct unbounded network calls lacking retry resilience wrappers.
```

**Accept when:**
- All outbound network probe handlers and background job runners execute external calls through bounded github.com/cenkalti/backoff retry structures.
- Project verification test suites confirm that transient network faults trigger exponential backoff retries and terminate upon reaching maximum retry limits or context deadlines.
- Static analysis checks pass across all modules without flagging unhandled or unbounded external network calls.

<enforcement>
Claude Code MUST NOT skip or defer verification. All outbound network requests must be validated against bounded exponential backoff retry rules and static analysis checks.
</enforcement>