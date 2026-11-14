---
version: 2
name: cross-cutting-network-probing-jobs-subjected-transient-811e
description: "github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Network Probing Jobs Subjected Transient Failure"
metadata:
  actual:
    adr_id: 811e651b-38fd-4453-9c5b-bc4bba1d1fcf
    status: accepted
    candidate_group_id: 396c4af6-a8e5-4905-8e51-6189c2c9fb2c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - pkg/job/**
---

# github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Network Probing Jobs Subjected Transient Failure

Scope: applies to `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** MUST: All network-probing jobs subjected to transient failure MUST orchestrate retries using github.com/cenkalti/backoff/v5 via backoff.Retry, configuring backoff.WithBackOff using backoff.NewExponentialBackOff and constraining retry counts via backoff.WithMaxTries.

### Verify

```bash
# Discover and execute the project's static analysis and linting scripts to verify adherence to retry API usage standards.
# Discover and execute the test runner across network probe packages to ensure retry resilience and context cancellation behave as expected.
```

**Accept when:**
- All checker jobs utilize backoff.Retry with backoff.WithMaxTries and backoff.NewExponentialBackOff for transient network calls.
- Automated test suites pass with zero failures and confirm retry exhaustion triggers expected errors within configured timeout limits.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks and mandatory peer code review.
</enforcement>