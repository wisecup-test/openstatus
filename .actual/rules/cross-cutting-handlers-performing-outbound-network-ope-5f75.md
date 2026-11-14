---
version: 2
name: cross-cutting-handlers-performing-outbound-network-ope-5f75
description: "Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Handlers Performing Outbound Network Operations Coordinate"
metadata:
  actual:
    adr_id: 5f751d3a-17ba-4103-931a-27effad9f41e
    status: accepted
    candidate_group_id: 415a5e14-8045-4dd9-b8d7-7c09343cc18c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - handlers/**
---

# Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Handlers Performing Outbound Network Operations Coordinate

Scope: applies to `apps/checker/handlers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** MUST: Handlers performing outbound network operations MUST coordinate transient failure retries using the adopted github.com/cenkalti/backoff/v4 library rather than bespoke sleep or loop constructs.

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
Claude Code MUST NOT skip or defer verification. Compliance with these rules is mandatory and enforced via automated tests, code review, and static analysis sweeps.
</enforcement>