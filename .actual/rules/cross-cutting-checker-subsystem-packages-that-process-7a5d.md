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

- **R-CHK-001** MUST: Checker subsystem packages that process, record, or evaluate network transactions MUST adopt github.com/openstatushq/openstatus/apps/checker/request as their canonical request and response domain representation.
- **R-CHK-002** MUST: Implementations adding new protocol support MUST expand the shared request module rather than creating isolated protocol representations.
- **R-CHK-003** MUST: Helper routines for assertion evaluation and metric extraction MUST consume read-only views or standard accessor methods provided by the shared module.

### Verify

```bash
# Discover workspace test runner scripts from the repository configuration and execute unit and integration test suites covering checker subsystem packages.
# Run project dependency and static analysis verification scripts to confirm import compliance and verify absence of circular dependencies.
```

**Accept when:**
- All checker subsystem packages compile cleanly while referencing the shared request module contracts.
- Automated test suites covering assertion evaluation and metric collection pass without schema incompatibility errors.
- Static dependency boundary checks confirm no circular imports exist between the shared request module and its consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews validate module dependency trees and test suites across all checker packages.
</enforcement>