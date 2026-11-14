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

These rules are ALWAYS ACTIVE for all background cron workflows, scheduled batch processing routines, and asynchronous task dispatch loops requiring outbound throughput throttling.

### Rules

- **R-RL-001** SHOULD: RateLimiter instances for recurring tasks SHOULD define interval durations aligned with downstream quota reset windows to prevent request throttling.
- **R-RL-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-RL-003** MANDATORY: Execute the lock-version grounding process in order before writing code that uses a versioned library: find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for exact resolved version, confirm API existence, and re-run per dependency at point of use.
- **R-RL-004** MANDATORY: Instantiate RateLimiter once across the lifecycle of the workflow execution runner rather than per task item to maintain continuous token consumption history.
- **R-RL-005** MANDATORY: Deduct tokens proportionally when an individual task step dispatches multiple downstream requests.

### Verify

```bash
# Discover the project test runner configuration from the repository manifest and execute the test suite covering background workflow rate limiting.
# Inspect the project linter and static analysis configuration to verify that RateLimiter instantiation adheres to token interval standards.
```

**Accept when:**
- Automated tests confirm that task processing adheres to configured token rates and delays burst execution exceeding capacity.
- Static analysis and type checking pass with zero violations regarding RateLimiter configuration and task pacing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites in continuous integration validate rate limiting behavior and timing constraints. Peer architectural code review is required for changes affecting background workflow task dispatch loops.
</enforcement>