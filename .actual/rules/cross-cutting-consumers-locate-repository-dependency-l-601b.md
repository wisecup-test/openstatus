---
version: 2
name: cross-cutting-consumers-locate-repository-dependency-l-601b
description: "github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Consumers Locate Repository Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 601bdcaa-79f3-471d-b5da-e4d782fe90dc
    status: accepted
    candidate_group_id: 396c4af6-a8e5-4905-8e51-6189c2c9fb2c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - pkg/job/**
---

# github.com/cenkalti/backoff/v5 for Exponential Retry Resilience: Consumers Locate Repository Dependency Lock Artifact

Scope: applies to `apps/checker/pkg/job/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BACKOFF-001** MUST: Consumers MUST locate the repository dependency lock artifact, verify the exact resolved version of github.com/cenkalti/backoff/v5, and confirm that all referenced API constructs exist in that resolved version documentation before authoring retry logic.
- **R-BACKOFF-002** MUST: Pass the invocation context directly into backoff.Retry to guarantee immediate termination when outer timeouts or cancellation signals occur.
- **R-BACKOFF-003** MUST: Always initialize backoff.NewExponentialBackOff inside backoff.WithBackOff to ensure proper initialization of jitter and backoff interval calculations.
- **R-BACKOFF-004** MUST: Strictly enforce backoff.WithMaxTries and root context cancellation timeouts on every backoff.Retry invocation to prevent unbounded backoff durations.
- **R-BACKOFF-005** MUST: Differentiate permanent errors within the operation closure to return permanent failure indicators where appropriate, avoiding retries on non-transient errors.

### Verify

```bash
# Discover and execute the project's static analysis and linting scripts to verify adherence to retry API usage standards.
# Discover and execute the test runner across network probe packages to ensure retry resilience and context cancellation behave as expected.
go test ./...
```

**Accept when:**
- All checker jobs utilize backoff.Retry with backoff.WithMaxTries and backoff.NewExponentialBackOff for transient network calls.
- Automated test suites pass with zero failures and confirm retry exhaustion triggers expected errors within configured timeout limits.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>