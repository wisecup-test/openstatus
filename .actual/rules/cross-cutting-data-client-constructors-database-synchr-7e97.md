---
version: 2
name: cross-cutting-data-client-constructors-database-synchr-7e97
description: "Internal env Module Adoption for Centralized Environment Configuration: Data Client Constructors Database Synchronization Handlers"
metadata:
  actual:
    adr_id: 7e97f026-618a-41a2-9822-687265239673
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

# Internal env Module Adoption for Centralized Environment Configuration: Data Client Constructors Database Synchronization Handlers

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`, `apps/workflows/src/utils/**`.

These rules are ALWAYS ACTIVE for all background workflow handlers, cron routines, HTTP route handlers, and infrastructure client adapters requiring runtime configuration.

### Rules

- **R-ENV-001** SHOULD: Data client constructors, database synchronization handlers, and external service clients SHOULD obtain connection credentials and API tokens directly from env() invocations.

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