---
version: 2
name: cross-cutting-automated-browser-operations-not-run-864c
description: "Browser Session Allocation with newPage: Automated Browser Operations Not Run Without"
metadata:
  actual:
    adr_id: 864c2e30-84f2-4a39-bbba-bc8bc723cf47
    status: accepted
    candidate_group_id: e4f70103-7cc0-4f23-bc97-139f2d3184be
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Browser Session Allocation with newPage: Automated Browser Operations Not Run Without

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for services executing automated browser navigation, visual capture, or end-to-end rendering workflows.

### Rules

- **R-BROWSER-001** SHOULD_NOT: Automated browser operations SHOULD NOT run without explicit navigation and execution timeout boundaries.

### Verify

```bash
# Discover the workspace test runner from the root configuration and execute the automated test suite.
# Discover the repository static analysis and linting script to check for unhandled asynchronous browser promises and resource leakage.
```

**Accept when:**
- All automated tests covering browser page allocation and teardown complete with zero failures.
- Static verification confirms that every page allocated through newPage has an associated closure call in its termination path.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>