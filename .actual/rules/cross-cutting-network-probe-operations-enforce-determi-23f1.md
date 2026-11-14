---
version: 2
name: cross-cutting-network-probe-operations-enforce-determi-23f1
description: "Adopt net Library for Network Diagnostics and Connectivity Probing: Network Probe Operations Enforce Deterministic Cancellation"
metadata:
  actual:
    adr_id: 23f1ca69-48a4-4f24-9fe8-4a4a70eb584a
    status: accepted
    candidate_group_id: 9f0ad357-be83-4024-8f58-796c214366fa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
---

# Adopt net Library for Network Diagnostics and Connectivity Probing: Network Probe Operations Enforce Deterministic Cancellation

Scope: applies to `apps/checker/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** MUST: Network probe operations MUST enforce deterministic cancellation and timeout boundaries on all connection and resolution calls via execution contexts.

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