---
version: 2
name: cross-cutting-network-boundary-operations-executing-ex-146b
description: "Resilience Retry Orchestration via github.com/cenkalti/backoff: Network Boundary Operations Executing External Health"
metadata:
  actual:
    adr_id: 146b3428-8435-4a0d-9cbe-c075dca35f60
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

# Resilience Retry Orchestration via github.com/cenkalti/backoff: Network Boundary Operations Executing External Health

Scope: applies to `apps/checker/handlers/**`, `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for outbound network operations, probe handlers, and automated diagnostic jobs dispatching requests across external network boundaries.

### Rules

- **R-RES-001** MUST: Network boundary operations executing external health, availability, or diagnostic checks MUST be wrapped within backoff.Retry operations utilizing github.com/cenkalti/backoff.

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
Claude Code MUST NOT skip or defer verification. Non-compliant external network calls must be rejected until compliant retry boundaries are established.
</enforcement>