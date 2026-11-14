---
version: 2
name: cross-cutting-assertion-evaluation-contracts-operate-d-9178
description: "Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Assertion Evaluation Contracts Operate Directly Upon"
metadata:
  actual:
    adr_id: 91787304-bb09-4904-8ffb-b3f8f961928d
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

# Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Assertion Evaluation Contracts Operate Directly Upon

Scope: applies to `apps/checker/pkg/assertions/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all packages and modules within the checker application that inspect, record, or evaluate network request and response data.

### Rules

- **R-REQ-001** MUST: Assertion evaluation contracts MUST operate directly upon payload, header, status, and record structures supplied by the shared request module.

### Verify

```bash
# Discover and execute workspace test runner scripts covering checker subsystem packages
# Run dependency and static analysis verification scripts to confirm import compliance and absence of circular dependencies
```

**Accept when:**
- All checker subsystem packages compile cleanly while referencing the shared request module contracts.
- Automated test suites covering assertion evaluation and metric collection pass without schema incompatibility errors.
- Static dependency boundary checks confirm no circular imports exist between the shared request module and its consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Enforcement is verified via automated continuous integration checks and peer code reviews.
</enforcement>