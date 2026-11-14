---
version: 2
name: cross-cutting-retry-operations-configure-explicit-maxi-589a
description: "Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Retry Operations Configure Explicit Maximum Count"
metadata:
  actual:
    adr_id: 589ae0bd-1d5b-421a-83bd-7b4a1c02424f
    status: accepted
    candidate_group_id: 415a5e14-8045-4dd9-b8d7-7c09343cc18c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - handlers/**
---

# Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Retry Operations Configure Explicit Maximum Count

Scope: applies to `apps/checker/handlers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RETRY-001** MUST: Retry operations MUST configure an explicit maximum retry count using backoff.WithMaxRetries combined with backoff.NewExponentialBackOff to prevent unbounded execution loops.

### Verify

```bash
# Discover and execute the project test runner for handler packages to validate exponential backoff behavior under simulated network faults.
# Discover and run the project static analysis and linting targets to ensure no unmanaged retry loops exist in handler implementations.
```

**Accept when:**
- All outbound HTTP client network calls that execute within request handlers coordinate transient failure recovery through the adopted backoff library.
- Automated verification scripts confirm zero unhandled transient network errors and verify that retry operations enforce configured maximum retry counts.
- Static analysis verifies that lock resolution artifacts match the active environment before building handler packages.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>