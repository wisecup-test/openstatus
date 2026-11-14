---
version: 2
name: cross-cutting-error-handling-network-operations-inspec-45f3
description: "Adopt net Library for Network Diagnostics and Connectivity Probing: Error Handling Network Operations Inspect Classify"
metadata:
  actual:
    adr_id: 45f3ed1c-c06a-4b7a-aaee-09470cbaeb23
    status: accepted
    candidate_group_id: 9f0ad357-be83-4024-8f58-796c214366fa
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
---

# Adopt net Library for Network Diagnostics and Connectivity Probing: Error Handling Network Operations Inspect Classify

Scope: applies to `apps/checker/checker/**`.

These rules are ALWAYS ACTIVE for all service modules and components performing network diagnostic probes, connection checks, protocol resolution, or exposing health check and latency measurement contracts.

### Rules

- **R-NET-001** SHOULD: Error handling in network operations SHOULD inspect and classify transport-level faults to distinguish network reachability errors from connection timeouts.
- **R-NET-002** MANDATORY: Enforce mandatory context-driven timeouts on all network socket dial and resolution operations.
- **R-NET-003** MANDATORY: Ensure all network connections and response streams are closed using deferred cleanup blocks.
- **R-NET-004** MANDATORY: Ensure context deadlines are passed directly to dialers and resolver queries to maintain consistent timeout behavior across all probe routines.
- **R-NET-005** MANDATORY: Encapsulate transport probe responses into structured return types that capture both latency metrics and error classifications.

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