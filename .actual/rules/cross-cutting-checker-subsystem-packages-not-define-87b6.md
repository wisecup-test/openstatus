---
version: 2
name: cross-cutting-checker-subsystem-packages-not-define-87b6
description: "Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Checker Subsystem Packages Not Define Independent"
metadata:
  actual:
    adr_id: 87b6afe7-f819-4baf-95a7-dbd0bb54ebf2
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

# Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Checker Subsystem Packages Not Define Independent

Scope: applies to `apps/checker/pkg/assertions/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all packages and modules within the checker application that inspect, record, or evaluate network request and response data.

### Rules

- **R-CHK-001** MUST_NOT: Checker subsystem packages MUST NOT define independent duplicate request or response data models for network transaction processing.

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
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews enforce compliance.
</enforcement>