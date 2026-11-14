---
version: 2
name: cross-cutting-before-implementing-components-that-cons-35f3
description: "Browser Session Allocation with newPage: Before Implementing Components That Consume Browser"
metadata:
  actual:
    adr_id: 35f317cd-2658-4481-ba46-8b019a741edb
    status: accepted
    candidate_group_id: e4f70103-7cc0-4f23-bc97-139f2d3184be
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Browser Session Allocation with newPage: Before Implementing Components That Consume Browser

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for services executing automated browser navigation, visual capture, or end-to-end rendering workflows.

### Rules

- **R-BROWSER-001** MUST: Before implementing components that consume browser automation dependencies, developers MUST discover the dependency declaration artifact and inspect the repository lock artifact to resolve the exact locked version.

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