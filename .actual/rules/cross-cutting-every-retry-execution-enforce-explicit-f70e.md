---
version: 2
name: cross-cutting-every-retry-execution-enforce-explicit-f70e
description: "Resilience Retry Orchestration via github.com/cenkalti/backoff: Every Retry Execution Enforce Explicit Upper"
metadata:
  actual:
    adr_id: f70ef47a-cc44-418e-a050-6f16c0dec3c8
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

# Resilience Retry Orchestration via github.com/cenkalti/backoff: Every Retry Execution Enforce Explicit Upper

Scope: applies to `apps/checker/handlers/**`, `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for outbound network operations, probe handlers, automated diagnostic jobs dispatching requests across external network boundaries, and network verification routines evaluating remote HTTP, DNS, TCP, and ICMP endpoints.

### Rules

- **R-RES-001** MUST: Every retry execution MUST enforce an explicit upper bound on attempts using either backoff.WithMaxRetries or backoff.WithMaxTries derived from request configuration or predefined boundary thresholds.

### Verify

```bash
# Discover and execute project dependency audit script to verify lock-file consistency for third-party resilience libraries
# Discover and run project test suite to ensure all network retry policies adhere to configured exponential backoff bounds and context cancellations
# Execute static analysis rules discovered from repository configuration to detect direct unbounded network calls lacking retry resilience wrappers
```

**Accept when:**
- All outbound network probe handlers and background job runners execute external calls through bounded github.com/cenkalti/backoff retry structures.
- Project verification test suites confirm that transient network faults trigger exponential backoff retries and terminate upon reaching maximum retry limits or context deadlines.
- Static analysis checks pass across all modules without flagging unhandled or unbounded external network calls.

<enforcement>
Claude Code MUST NOT skip or defer verification. All outbound network operations must be verified through automated static analysis, peer code reviews, and continuous integration pipeline controls.
</enforcement>