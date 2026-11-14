---
version: 2
name: cross-cutting-implementations-combine-process-ratelimi-6057
description: "RateLimiter Token Bucket Rate Limiting for Background Workflows: Implementations Combine Process Ratelimiter Token Buckets"
metadata:
  actual:
    adr_id: 60577f58-2b47-4dec-9332-2c9b85972d6f
    status: accepted
    candidate_group_id: c79a4da2-7cea-4021-880b-d93603b2b83e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# RateLimiter Token Bucket Rate Limiting for Background Workflows: Implementations Combine Process Ratelimiter Token Buckets

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for background monitoring workflows, scheduled batch processing routines, and asynchronous task dispatch loops requiring outbound throughput throttling.

### Rules

- **R-RL-001** MAY: Implementations MAY combine in-process RateLimiter token buckets with distributed caching key-value storage when cross-worker coordination is required.
- **R-RL-002** MANDATORY: Discover the project test runner configuration from the repository manifest and execute the test suite covering background workflow rate limiting.
- **R-RL-003** MANDATORY: Inspect the project linter and static analysis configuration to verify that RateLimiter instantiation adheres to token interval standards.
- **R-RL-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact resolved version, (4) Look up official documentation for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-RL-005** MANDATORY: Instantiate RateLimiter once across the lifecycle of the workflow execution runner rather than per task item to maintain continuous token consumption history.
- **R-RL-006** MANDATORY: Deduct tokens proportionally when an individual task step dispatches multiple downstream requests.

### Verify

```bash
# Discover and run the project test runner configuration covering background workflow rate limiting
# (Command discovered dynamically from repository manifest)
```

**Accept when:**
- Automated tests confirm that task processing adheres to configured token rates and delays burst execution exceeding capacity.
- Static analysis and type checking pass with zero violations regarding RateLimiter configuration and task pacing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites in continuous integration validating rate limiting behavior and timing constraints, and peer architectural code review. Violation handling: Pull requests introducing unthrottled batch operations against external APIs or database tables will be blocked during code review, and CI verification failures will halt deployment pipelines.
</enforcement>