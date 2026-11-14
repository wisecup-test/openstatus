---
version: 2
name: cross-cutting-browser-automation-contexts-not-share-fd2d
description: "Browser Session Allocation with newPage: Browser Automation Contexts Not Share State"
metadata:
  actual:
    adr_id: fd2d1e9d-7153-4b0e-b9dd-c7b1c1aacc51
    status: accepted
    candidate_group_id: e4f70103-7cc0-4f23-bc97-139f2d3184be
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Browser Session Allocation with newPage: Browser Automation Contexts Not Share State

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for services executing automated browser navigation, visual capture, or end-to-end rendering workflows.

### Rules

- **R-BRA-001** MUST_NOT: Browser automation contexts MUST NOT share state, cookies, or cached page sessions across distinct execution tasks.

### Verify

```bash
# Discover the workspace test runner from root configuration and run automated tests
# Discover the repository static analysis and linting script to check for unhandled asynchronous browser promises and resource leakage
```

**Accept when:**
- All automated tests covering browser page allocation and teardown complete with zero failures.
- Static verification confirms that every page allocated through newPage has an associated closure call in its termination path.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>