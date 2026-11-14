---
version: 2
name: cross-cutting-ratelimiter-instances-recurring-tasks-de-f8a0
description: "RateLimiter Token Bucket Rate Limiting for Background Workflows: Ratelimiter Instances Recurring Tasks Define Interval"
metadata:
  actual:
    adr_id: f8a06049-c34e-407b-acc3-a6e1b03037a8
    status: accepted
    candidate_group_id: c79a4da2-7cea-4021-880b-d93603b2b83e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# RateLimiter Token Bucket Rate Limiting for Background Workflows: Ratelimiter Instances Recurring Tasks Define Interval

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for background cron workflows, scheduled batch processing routines, and asynchronous task dispatch loops requiring outbound throughput throttling.

### Rules

- **R-RATE-001** SHOULD: RateLimiter instances for recurring tasks SHOULD define interval durations aligned with downstream quota reset windows to prevent request throttling.
- **R-RATE-002** MANDATORY: Instantiate RateLimiter once across the lifecycle of the workflow execution runner rather than per task item to maintain continuous token consumption history.
- **R-RATE-003** MANDATORY: Deduct tokens proportionally when an individual task step dispatches multiple downstream requests.

### Verify

```bash
# Discover the project test runner configuration from the repository manifest and execute the test suite covering background workflow rate limiting.
# Inspect the project linter and static analysis configuration to verify that RateLimiter instantiation adheres to token interval standards.
```

**Accept when:**
- Automated tests confirm that task processing adheres to configured token rates and delays burst execution exceeding capacity.
- Static analysis and type checking pass with zero violations regarding RateLimiter configuration and task pacing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>