---
version: 2
name: cross-cutting-application-code-not-access-ambient-27be
description: "Internal env Module Adoption for Centralized Environment Configuration: Application Code Not Access Ambient Runtime"
metadata:
  actual:
    adr_id: 27be9c80-595c-4408-a9a5-508d0e437a53
    status: accepted
    candidate_group_id: 7c1d83aa-a233-4b90-af47-a3f448078481
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
      - src/incident/**
      - src/lib/**
      - src/scripts/**
      - src/utils/**
---

# Internal env Module Adoption for Centralized Environment Configuration: Application Code Not Access Ambient Runtime

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`, `apps/workflows/src/utils/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST_NOT: Application code MUST NOT access ambient runtime environment variables or process globals directly outside the centralized env module.

### Verify

```bash
# Discover the project task runner or verification scripts from the project configuration and execute static analysis checks against workflow modules.
# Discover the project test runner and execute the test suite to verify configuration retrieval and service initialization behavior.
```

**Accept when:**
- Static analysis confirms all environment access within workflow and background modules routes exclusively through the internal env module.
- Automated test executions pass with validated environment configurations across all service routes and cron tasks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>