---
version: 2
name: cross-cutting-network-diagnostic-modules-not-introduce-c896
description: "Adopt net Library for Network Diagnostics and Connectivity Probing: Network Diagnostic Modules Not Introduce Third"
metadata:
  actual:
    adr_id: c8968e5f-64e7-40f7-99df-bf3cf5006bd2
    status: accepted
    candidate_group_id: 9f0ad357-be83-4024-8f58-796c214366fa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
---

# Adopt net Library for Network Diagnostics and Connectivity Probing: Network Diagnostic Modules Not Introduce Third

Scope: applies to `apps/checker/checker/**`.

These rules are ALWAYS ACTIVE for all service modules and components performing network diagnostic probes, connection checks, protocol resolution, and diagnostic endpoints exposing health check and latency measurement contracts.

### Rules

- **R-NET-001** MUST_NOT: Network diagnostic modules MUST NOT introduce third-party network client dependencies when transport and protocol capabilities are supported by the net library.

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