---
version: 2
name: cross-cutting-scheduled-monitor-jobs-not-execute-afc9
description: "RateLimiter Token Bucket Rate Limiting for Background Workflows: Scheduled Monitor Jobs Not Execute Unbounded"
metadata:
  actual:
    adr_id: afc96b29-eabd-4526-8e43-52ad74014170
    status: accepted
    candidate_group_id: c79a4da2-7cea-4021-880b-d93603b2b83e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# RateLimiter Token Bucket Rate Limiting for Background Workflows: Scheduled Monitor Jobs Not Execute Unbounded

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for background cron workflows, scheduled batch processing routines, and asynchronous task dispatch loops requiring outbound throughput throttling.

### Rules

- **R-RATELIMIT-001** MUST_NOT: Scheduled monitor jobs MUST_NOT execute unbounded parallel operations against external APIs, task dispatch queues, or database access layers without acquiring rate tokens from RateLimiter.

### Verify

```bash
# Discover the project test runner configuration from the repository manifest and execute the test suite covering background workflow rate limiting.
# Inspect the project linter and static analysis configuration to verify that RateLimiter instantiation adheres to token interval standards.
```

**Accept when:**
- Automated tests confirm that task processing adheres to configured token rates and delays burst execution exceeding capacity.
- Static analysis and type checking pass with zero violations regarding RateLimiter configuration and task pacing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites and peer architectural code review.
</enforcement>