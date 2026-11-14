---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-resolut-6c4c
description: "Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Consumer Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 6c4c2f0a-f0d8-40da-93e3-1e4d00795dd3
    status: accepted
    candidate_group_id: 415a5e14-8045-4dd9-b8d7-7c09343cc18c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - handlers/**
---

# Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Consumer Inspect Repository Lock Resolution Artifact

Scope: applies to `apps/checker/handlers/**`.

These rules are ALWAYS ACTIVE for all network client request handlers executing outbound diagnostic checks, regional ping operations, and transient network error recovery.

### Rules

- **R-BO-001** MUST: The consumer MUST inspect the repository lock or resolution artifact to determine the exact resolved version of the backoff dependency and verify API compatibility before use.
- **R-BO-002** MUST: Wrap target HTTP operations inside an operation closure that returns an error when transient failure conditions are encountered.
- **R-BO-003** MUST: Ensure backoff configuration limits both total attempts and total duration to prevent handler context exhaustion.

### Verify

```bash
# Discover and execute the project test runner for handler packages to validate exponential backoff behavior under simulated network faults
# Discover and run the project static analysis and linting targets to ensure no unmanaged retry loops exist in handler implementations
```

**Accept when:**
- All outbound HTTP client network calls that execute within request handlers coordinate transient failure recovery through the adopted backoff library.
- Automated verification scripts confirm zero unhandled transient network errors and verify that retry operations enforce configured maximum retry counts.
- Static analysis verifies that lock resolution artifacts match the active environment before building handler packages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated test suites, code review verification, and static analysis sweeps checking for direct unmanaged network retry loops.
</enforcement>