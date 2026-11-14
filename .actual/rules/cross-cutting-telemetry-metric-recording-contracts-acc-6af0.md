---
version: 2
name: cross-cutting-telemetry-metric-recording-contracts-acc-6af0
description: "Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Telemetry Metric Recording Contracts Accept Extract"
metadata:
  actual:
    adr_id: 6af04719-bcdb-4acb-89a6-8c03fe708a19
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

# Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Telemetry Metric Recording Contracts Accept Extract

Scope: applies to `apps/checker/pkg/assertions/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all packages and modules within the checker application that inspect, record, or evaluate network request and response data.

### Rules

- **R-TEL-001** MUST: Telemetry metric recording contracts MUST accept and extract operational metrics directly from the shared request data structures.

### Verify

```bash
# Discover workspace test runner scripts and execute unit/integration tests covering checker subsystem packages
# Run project dependency and static analysis verification scripts to confirm import compliance and absence of circular dependencies
```

**Accept when:**
- All checker subsystem packages compile cleanly while referencing the shared request module contracts.
- Automated test suites covering assertion evaluation and metric collection pass without schema incompatibility errors.
- Static dependency boundary checks confirm no circular imports exist between the shared request module and its consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes must pass automated continuous integration checks and peer code review validating module dependency trees and test suites across all checker packages.
</enforcement>