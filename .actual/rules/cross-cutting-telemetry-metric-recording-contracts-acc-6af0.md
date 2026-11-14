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
- **R-TEL-002** MUST: Implementations adding new protocol support must expand the shared request module rather than creating isolated protocol representations.
- **R-TEL-003** MUST: Helper routines for assertion evaluation and metric extraction must consume read-only views or standard accessor methods provided by the shared module.
- **R-TEL-004** MUST: Before writing code that uses a versioned library, developers and automated agents must find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact to determine the exact resolved version, look up official documentation for that exact version, and confirm every API, class, or function exists in that version before using it.

### Verify

```bash
# Discover and execute workspace test runner scripts covering checker subsystem packages
# Run project dependency and static analysis verification scripts to confirm import compliance and absence of circular dependencies
```

**Accept when:**
- All checker subsystem packages compile cleanly while referencing the shared request module contracts.
- Automated test suites covering assertion evaluation and metric collection pass without schema incompatibility errors.
- Static dependency boundary checks confirm no circular imports exist between the shared request module and its consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying imports or defining request-handling contracts within the checker application will be validated by automated CI checks and peer code review.
</enforcement>