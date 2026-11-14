---
version: 2
name: cross-cutting-background-cron-jobs-monitoring-alert-0bd5
description: "Adoption of Effect Library for Workflow Services: Background Cron Jobs Monitoring Alert Tasks"
metadata:
  actual:
    adr_id: 0bd545af-f062-4622-a90d-264e5e6b07a2
    status: accepted
    candidate_group_id: 8f23a55a-0b96-4d0e-88b7-ec6fbdab8372
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/checker/**
      - src/cron/**
---

# Adoption of Effect Library for Workflow Services: Background Cron Jobs Monitoring Alert Tasks

Scope: applies to `apps/workflows/src/checker/**`, `apps/workflows/src/cron/**`.

These rules are ALWAYS ACTIVE for asynchronous workflow task handlers, scheduled cron job processors, and background incident and alerting dispatchers.

### Rules

- **R-WFS-001** SHOULD: Background cron jobs and monitoring alert tasks SHOULD encapsulate external service calls and database transactions within effect execution pipelines to maintain composability.

### Verify

```bash
# Discover and execute the project verification script defined in the dependency manifest
# Discover and run static type checking to verify compliance of effect pipeline signatures
# Inspect lock resolution artifacts to verify the resolved version of effect matches documented APIs
```

**Accept when:**
- The project type verification and static analysis checks pass with zero diagnostic errors across workflow modules.
- All asynchronous workflow handlers return managed effect computations without unhandled promise rejections.
- The resolved version of effect in the repository lock artifact matches the API surface used in workflow code.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via static analysis and automated type check suites executed during continuous integration.
</enforcement>