---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-4408
description: "Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Consumers Inspect Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 44080525-fd36-4c6b-8264-4cddb3efd474
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

# Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Consumers Inspect Repository Dependency Manifest Lock

Scope: applies to `apps/checker/pkg/assertions/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all packages and modules within the checker application that inspect, record, or evaluate network request and response data.

### Rules

- **R-REQ-001** MUST: Consumers MUST inspect the repository dependency manifest and lock artifact to resolve and verify exact locked versions before implementing or updating code using versioned dependencies.

### Verify

```bash
# Discover workspace test runner scripts from the repository configuration and execute unit and integration test suites covering checker subsystem packages.
# Run the project dependency and static analysis verification scripts to confirm import compliance and verify the absence of circular dependencies.
```

**Accept when:**
- All checker subsystem packages compile cleanly while referencing the shared request module contracts.
- Automated test suites covering assertion evaluation and metric collection pass without schema incompatibility errors.
- Static dependency boundary checks confirm no circular imports exist between the shared request module and its consumers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying imports or defining request-handling contracts within the checker application will be verified by automated continuous integration checks and peer code reviews.
</enforcement>