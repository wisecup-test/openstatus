---
version: 2
name: cross-cutting-before-implementing-updating-network-pro-9214
description: "Adopt net Library for Network Diagnostics and Connectivity Probing: Before Implementing Updating Network Probing Modules"
metadata:
  actual:
    adr_id: 92149c86-157b-4fd2-a8bf-8860a70bb0cf
    status: accepted
    candidate_group_id: 9f0ad357-be83-4024-8f58-796c214366fa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
---

# Adopt net Library for Network Diagnostics and Connectivity Probing: Before Implementing Updating Network Probing Modules

Scope: applies to `apps/checker/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** MUST: Before implementing or updating network probing modules, the consumer MUST inspect repository dependency manifests and lock artifacts to resolve exact runtime versions and ensure compatibility.

### Verify

```bash
discover_project_test_runner && run_project_tests
discover_project_linter && run_project_linter
```

**Accept when:**
- Network diagnostic and connectivity probe routines pass all unit and integration test suites.
- Static code analysis confirms network probe implementations consume the net library without unapproved third-party networking wrappers.
- All network operations demonstrate deterministic timeout enforcement under simulated connection failure conditions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>