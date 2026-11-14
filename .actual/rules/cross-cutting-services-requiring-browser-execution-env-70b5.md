---
version: 2
name: cross-cutting-services-requiring-browser-execution-env-70b5
description: "Browser Session Allocation with newPage: Services Requiring Browser Execution Environments Instantiate"
metadata:
  actual:
    adr_id: 70b51b3a-ce3d-4751-b544-ea8e9eaf5373
    status: accepted
    candidate_group_id: e4f70103-7cc0-4f23-bc97-139f2d3184be
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Browser Session Allocation with newPage: Services Requiring Browser Execution Environments Instantiate

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-BS-001** MUST: Services requiring browser execution environments MUST instantiate isolated browser sessions via the newPage interface to ensure lifecycle boundary separation.

### Verify

```bash
# Discover workspace test runner and execute automated test suite
# Discover repository static analysis and linting script to check for unhandled asynchronous browser promises and resource leakage
```

**Accept when:**
- All automated tests covering browser page allocation and teardown complete with zero failures.
- Static verification confirms that every page allocated through newPage has an associated closure call in its termination path.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>