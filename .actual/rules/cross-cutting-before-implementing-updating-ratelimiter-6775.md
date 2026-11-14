---
version: 2
name: cross-cutting-before-implementing-updating-ratelimiter-6775
description: "RateLimiter Token Bucket Rate Limiting for Background Workflows: Before Implementing Updating Ratelimiter Dependencies Engineers"
metadata:
  actual:
    adr_id: 67751ae4-5813-4410-a7d6-f2c8b9bde337
    status: accepted
    candidate_group_id: c79a4da2-7cea-4021-880b-d93603b2b83e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# RateLimiter Token Bucket Rate Limiting for Background Workflows: Before Implementing Updating Ratelimiter Dependencies Engineers

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for background monitoring workflows, scheduled polling routines, batch processing tasks, and asynchronous task dispatch loops requiring outbound throughput throttling.

### Rules

- **R-RL-001** MUST: Before implementing or updating RateLimiter dependencies, engineers MUST discover the project dependency manifest, inspect the authoritative lock artifact to verify the exact resolved version, and confirm API compatibility in official reference documentation.
- **R-RL-002** MUST: Instantiate RateLimiter once across the lifecycle of the workflow execution runner rather than per task item to maintain continuous token consumption history.
- **R-RL-003** MUST: Deduct tokens proportionally when an individual task step dispatches multiple downstream requests.
- **R-RL-004** SHOULD: Configure RateLimiter with explicit token intervals to provide predictable throughput pacing across batch cron cycles, avoiding sudden bursts against downstream services.

### Verify

```bash
# Discover dependency manifest, inspect lock artifact, and execute test suite covering background workflow rate limiting
# (Run project-specific dependency verification and test runner commands as derived from repository manifest)
```

**Accept when:**
- Automated tests confirm that task processing adheres to configured token rates and delays burst execution exceeding capacity.
- Static analysis and type checking pass with zero violations regarding RateLimiter configuration and task pacing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated test suites and architectural code review.
</enforcement>