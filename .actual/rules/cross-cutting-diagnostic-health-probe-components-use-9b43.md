---
version: 2
name: cross-cutting-diagnostic-health-probe-components-use-9b43
description: "Adopt net Library for Network Diagnostics and Connectivity Probing: Diagnostic Health Probe Components Use Net"
metadata:
  actual:
    adr_id: 9b434aa0-2a87-41f3-a5b4-2c4475de7904
    status: accepted
    candidate_group_id: 9f0ad357-be83-4024-8f58-796c214366fa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
---

# Adopt net Library for Network Diagnostics and Connectivity Probing: Diagnostic Health Probe Components Use Net

Scope: applies to `apps/checker/checker/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** MUST: Diagnostic and health probe components MUST use the net library for network transport, connection establishment, and protocol resolution operations.

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