---
version: 2
name: cross-cutting-browser-pages-created-through-newpage-bf7b
description: "Browser Session Allocation with newPage: Browser Pages Created Through Newpage Explicitly"
metadata:
  actual:
    adr_id: bf7bf02a-45ff-4f65-b71f-9864bbce993e
    status: accepted
    candidate_group_id: e4f70103-7cc0-4f23-bc97-139f2d3184be
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Browser Session Allocation with newPage: Browser Pages Created Through Newpage Explicitly

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for services executing automated browser navigation, visual capture, or end-to-end rendering workflows.

### Rules

- **R-BROWSER-001** SHOULD: Browser pages created through newPage SHOULD be explicitly closed in a finally block to release runtime resources.

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