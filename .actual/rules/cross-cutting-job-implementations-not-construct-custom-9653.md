---
version: 2
name: cross-cutting-job-implementations-not-construct-custom-9653
description: "github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Job Implementations Not Construct Custom Retry"
metadata:
  actual:
    adr_id: 9653e711-d8c3-4d95-acb3-7a8a3d6d159f
    status: accepted
    candidate_group_id: 396c4af6-a8e5-4905-8e51-6189c2c9fb2c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - pkg/job/**
---

# github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Job Implementations Not Construct Custom Retry

Scope: applies to `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-JOB-001** MUST_NOT: Job implementations MUST_NOT construct custom retry loops, sleep-based retry mechanisms, or uncoordinated linear backoff algorithms.

### Verify

```bash
# Discover and execute the project's static analysis and linting scripts to verify adherence to retry API usage standards
# Discover and execute the test runner across network probe packages to ensure retry resilience and context cancellation behave as expected
```

**Accept when:**
- All checker jobs utilize backoff.Retry with backoff.WithMaxTries and backoff.NewExponentialBackOff for transient network calls.
- Automated test suites pass with zero failures and confirm retry exhaustion triggers expected errors within configured timeout limits.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis checks and mandatory peer code reviews enforce these patterns in the continuous integration pipeline.
</enforcement>