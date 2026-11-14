---
version: 2
name: cross-cutting-network-probing-operations-capture-retur-921c
description: "Adopt net Library for Network Diagnostics and Connectivity Probing: Network Probing Operations Capture Return Structured"
metadata:
  actual:
    adr_id: 921c7597-3566-481f-bb9a-61f75611b7de
    status: accepted
    candidate_group_id: 9f0ad357-be83-4024-8f58-796c214366fa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
---

# Adopt net Library for Network Diagnostics and Connectivity Probing: Network Probing Operations Capture Return Structured

Scope: applies to `apps/checker/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** SHOULD: Network probing operations capture and return structured telemetry encompassing execution timing, protocol status, and resolution details.

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