---
version: 2
name: cross-cutting-checker-subsystem-packages-that-process-7a5d
description: "Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Checker Subsystem Packages That Process Record"
metadata:
  actual:
    adr_id: 7a5d5dbe-0a51-4772-93d0-03cebb6a3965
    status: accepted
    candidate_group_id: 8890ae36-f253-45a2-a72a-0cb76b733ce3
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - pkg/assertions/**
      - pkg/otel/**
---

# Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Checker Subsystem Packages That Process Record

Scope: applies to `apps/checker/pkg/assertions/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all packages and modules within the checker application that inspect, record, or evaluate network request and response data.

### Rules

- **R-CHECKER-001** MUST: Checker subsystem packages that process, record, or evaluate network transactions MUST adopt github.com/openstatushq/openstatus/apps/checker/request as their canonical request and response domain representation.

### Verify

```bash
# Discover the workspace test runner scripts from the repository configuration and execute unit and integration test suites covering checker subsystem packages.
# Run the project dependency and static analysis verification scripts to confirm import compliance and verify the absence of circular dependencies.
```

**Accept when:**
- All checker subsystem packages compile cleanly while referencing the shared request module contracts.
- Automated test suites covering assertion evaluation and metric collection pass without schema incompatibility errors.
- Static dependency boundary checks confirm no circular imports exist between the shared request module and its consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All checker subsystem packages must be verified via automated CI checks, peer code review, and static dependency boundary analysis.
</enforcement>