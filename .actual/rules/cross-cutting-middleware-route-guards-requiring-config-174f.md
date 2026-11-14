---
version: 2
name: cross-cutting-middleware-route-guards-requiring-config-174f
description: "Internal env Module Adoption for Centralized Environment Configuration: Middleware Route Guards Requiring Configuration Secrets"
metadata:
  actual:
    adr_id: 174f1694-1a32-4abe-8db3-9faa30b60515
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

# Internal env Module Adoption for Centralized Environment Configuration: Middleware Route Guards Requiring Configuration Secrets

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/incident/**`, `apps/workflows/src/lib/**`, `apps/workflows/src/scripts/**`, `apps/workflows/src/utils/**`.

These rules are ALWAYS ACTIVE for all background workflow handlers, cron routines, HTTP route handlers, and infrastructure client adapters requiring runtime configuration, as well as internal service scripts and database synchronization utilities.

### Rules

- **R-ENV-001** SHOULD: Middleware and route guards requiring configuration secrets for request authentication or authorization SHOULD retrieve the required secrets through the env() accessor function during request evaluation.

### Verify

```bash
# Discover and execute project static analysis checks
# Discover and execute project test runner
```

**Accept when:**
- Static analysis confirms all environment access within workflow and background modules routes exclusively through the internal env module.
- Automated test executions pass with validated environment configurations across all service routes and cron tasks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Direct reads of runtime environment globals will be blocked by static analysis and peer code review.
</enforcement>