---
version: 2
name: cross-cutting-errors-returned-failed-attempts-within-cb03
description: "github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Errors Returned Failed Attempts Within Operation"
metadata:
  actual:
    adr_id: cb031e61-597d-4111-b813-c2c57f9793a8
    status: accepted
    candidate_group_id: 396c4af6-a8e5-4905-8e51-6189c2c9fb2c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - pkg/job/**
---

# github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Errors Returned Failed Attempts Within Operation

Scope: applies to `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RETRY-001** SHOULD: Errors returned from failed attempts within the operation closure SHOULD be recorded in operational error logs when individual attempts fail prior to exhaustion of the retry quota.

### Verify

```bash
# Discover and execute project static analysis / linting scripts for retry API usage standards
# Discover and execute test runner across network probe packages to ensure retry resilience and context cancellation behave as expected
```

**Accept when:**
- All checker jobs utilize backoff.Retry with backoff.WithMaxTries and backoff.NewExponentialBackOff for transient network calls.
- Automated test suites pass with zero failures and confirm retry exhaustion triggers expected errors within configured timeout limits.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>