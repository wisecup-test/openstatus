---
version: 2
name: cross-cutting-retry-operations-configure-exponential-b-6938
description: "Resilience Retry Orchestration via github.com/cenkalti/backoff: Retry Operations Configure Exponential Backoff Schedule"
metadata:
  actual:
    adr_id: 69385e57-16c6-4eb5-9658-617889824ac0
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

# Resilience Retry Orchestration via github.com/cenkalti/backoff: Retry Operations Configure Exponential Backoff Schedule

Scope: applies to `apps/checker/handlers/**`, `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for outbound network operations, probe handlers, and automated diagnostic jobs dispatching requests across external network boundaries.

### Rules

- **R-RES-001** MUST: Retry operations MUST configure an exponential backoff schedule using backoff.NewExponentialBackOff to calculate interval delays between successive attempts.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>