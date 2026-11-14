---
version: 2
name: cross-cutting-background-monitoring-workflows-that-dis-88b8
description: "RateLimiter Token Bucket Rate Limiting for Background Workflows: Background Monitoring Workflows That Dispatch Recurring"
metadata:
  actual:
    adr_id: 88b8b2cd-6073-45e3-a97b-ce5cdc20a327
    status: accepted
    candidate_group_id: c79a4da2-7cea-4021-880b-d93603b2b83e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
---

# RateLimiter Token Bucket Rate Limiting for Background Workflows: Background Monitoring Workflows That Dispatch Recurring

Scope: applies to `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for background monitoring workflows that dispatch recurring operations, background cron workflows, scheduled batch processing routines, and asynchronous task dispatch loops requiring outbound throughput throttling.

### Rules

- **R-RATE-001** MUST: Background monitoring workflows that dispatch recurring operations MUST enforce token-bucket throughput constraints by instantiating RateLimiter with explicit tokensPerInterval and interval configurations.
- **R-RATE-002** MUST (DISCOVERY POLICY): Omit all tool names, file names, commands, package managers, and version numbers in ADR implementation; the consumer MUST derive them from the project repository.
- **R-RATE-003** MUST (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact resolved version, (4) Look up official documentation/changelog for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-RATE-004** MUST: Instantiate RateLimiter once across the lifecycle of the workflow execution runner rather than per task item to maintain continuous token consumption history.
- **R-RATE-005** MUST: Deduct tokens proportionally when an individual task step dispatches multiple downstream requests.

### Verify

```bash
# Discover the project test runner configuration from the repository manifest and execute the test suite covering background workflow rate limiting.
# Inspect the project linter and static analysis configuration to verify that RateLimiter instantiation adheres to token interval standards.
```

**Accept when:**
- Automated tests confirm that task processing adheres to configured token rates and delays burst execution exceeding capacity.
- Static analysis and type checking pass with zero violations regarding RateLimiter configuration and task pacing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites and peer architectural code review validate rate limiting behavior and timing constraints.
</enforcement>