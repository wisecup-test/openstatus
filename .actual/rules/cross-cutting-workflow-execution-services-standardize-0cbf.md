---
version: 2
name: cross-cutting-workflow-execution-services-standardize-0cbf
description: "Adoption of @sentry/deno for Workflow Exception Telemetry: Workflow Execution Services Standardize Sentry Deno"
metadata:
  actual:
    adr_id: 0cbf67ad-299e-4a73-b20f-f5c84406e084
    status: accepted
    candidate_group_id: 0a970db4-4f7d-45c5-a8bb-cf5b2fe6222e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/cron/**
      - src/lib/**
---

# Adoption of @sentry/deno for Workflow Exception Telemetry: Workflow Execution Services Standardize Sentry Deno

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** MUST: Workflow execution services MUST standardize on @sentry/deno as the exception tracking and error reporting client across all runtime workflow execution contexts.

### Verify

```bash
# Discover the project workspace test runner and execute the automated test suite covering workflow error handling and exception capture.
# Discover the project linter and static analysis runner, then execute code validation against workflow telemetry integrations.
```

**Accept when:**
- The automated test suite verifies that error handling routines invoke exception capture with required contextual tags.
- Static analysis and dependency verification confirm that error tracking imports resolve exclusively to the adopted runtime SDK without unverified version discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>