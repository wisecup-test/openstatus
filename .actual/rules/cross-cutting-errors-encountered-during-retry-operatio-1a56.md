---
version: 2
name: cross-cutting-errors-encountered-during-retry-operatio-1a56
description: "Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Errors Encountered During Retry Operation Execution"
metadata:
  actual:
    adr_id: 1a567cf6-0fdb-4dcd-92f6-e5b774247daa
    status: accepted
    candidate_group_id: 415a5e14-8045-4dd9-b8d7-7c09343cc18c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - handlers/**
---

# Adoption of github.com/cenkalti/backoff/v4 for Exponential Retry Operations: Errors Encountered During Retry Operation Execution

Scope: applies to `apps/checker/handlers/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RE-001** SHOULD: Errors encountered during retry operation execution SHOULD be recorded with contextual logging before or upon final operation failure.

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