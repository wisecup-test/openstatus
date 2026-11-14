---
version: 2
name: cross-cutting-when-execution-context-available-within-66b1
description: "Resilience Retry Orchestration via github.com/cenkalti/backoff: When Execution Context Available Within Calling"
metadata:
  actual:
    adr_id: 66b11aaa-024c-46d4-b59d-23fac0be216f
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

# Resilience Retry Orchestration via github.com/cenkalti/backoff: When Execution Context Available Within Calling

Scope: applies to `apps/checker/handlers/**`, `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for outbound network operations, probe handlers, automated diagnostic jobs dispatching requests across external network boundaries, and network verification routines evaluating remote HTTP, DNS, TCP, and ICMP endpoints.

### Rules

- **R-RES-001** SHOULD: When an execution context is available within the calling boundary, retry invocations SHOULD pass the cancellation context directly to backoff.Retry to ensure prompt termination upon deadline expiration or upstream cancellation.

### Verify

```bash
# Discover the project dependency audit script from repository configuration and execute it to verify lock-file consistency for third-party resilience libraries.
# Discover and run the project test suite to ensure all network retry policies adhere to configured exponential backoff bounds and context cancellations.
# Execute static analysis rules discovered from repository configuration to detect direct unbounded network calls lacking retry resilience wrappers.
```

**Accept when:**
- All outbound network probe handlers and background job runners execute external calls through bounded github.com/cenkalti/backoff retry structures.
- Project verification test suites confirm that transient network faults trigger exponential backoff retries and terminate upon reaching maximum retry limits or context deadlines.
- Static analysis checks pass across all modules without flagging unhandled or unbounded external network calls.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, code linters, and peer code reviews.
</enforcement>