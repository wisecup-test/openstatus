---
version: 2
name: cross-cutting-modifications-data-types-within-github-4aae
description: "Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Modifications Data Types Within Github Com"
metadata:
  actual:
    adr_id: 4aae6334-073d-42ef-a761-8eacfe414ef5
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

# Adoption of github.com/openstatushq/openstatus/apps/checker/request Internal Module: Modifications Data Types Within Github Com

Scope: applies to `apps/checker/pkg/assertions/**`, `apps/checker/pkg/otel/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MOD-001** SHOULD: Modifications to data types within github.com/openstatushq/openstatus/apps/checker/request maintain backward compatibility with existing telemetry and evaluation consumers.
- **R-MOD-002** MANDATORY: Implementations adding new protocol support expand the shared request module rather than creating isolated protocol representations.
- **R-MOD-003** MANDATORY: Helper routines for assertion evaluation and metric extraction consume read-only views or standard accessor methods provided by the shared module.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>