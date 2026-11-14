---
version: 2
name: cross-cutting-application-workflow-modules-route-handl-aaab
description: "Internal env Module Adoption for Centralized Environment Configuration: Application Workflow Modules Route Handlers Background"
metadata:
  actual:
    adr_id: aaabfd76-3ce9-4fcc-94c0-d0580bb34e62
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

# Internal env Module Adoption for Centralized Environment Configuration: Application Workflow Modules Route Handlers Background

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`, `apps/workflows/src/utils/**`.

These rules are ALWAYS ACTIVE for all application workflow modules, route handlers, background tasks, and service client initializations requiring runtime configuration.

### Rules

- **R-ENV-001** MUST: All application workflow modules, route handlers, background tasks, and service client initializations MUST access runtime environment configuration exclusively through the internal env module function env().

### Verify

```bash
# Discover and execute project static analysis checks for workflow modules
# Discover and execute the project test suite to verify configuration retrieval
```

**Accept when:**
- Static analysis confirms all environment access within workflow and background modules routes exclusively through the internal env module.
- Automated test executions pass with validated environment configurations across all service routes and cron tasks.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing direct reads of runtime environment globals will be blocked, and static analysis failures in automated pipelines will prevent merging non-compliant code.
</enforcement>