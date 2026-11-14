---
version: 2
name: cross-cutting-diagnostic-details-specific-failure-payl-27a4
description: "Adoption of @sentry/deno for Workflow Exception Telemetry: Diagnostic Details Specific Failure Payload External"
metadata:
  actual:
    adr_id: 27a4a34a-e405-4b0f-9f97-5aff2d8c531c
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

# Adoption of @sentry/deno for Workflow Exception Telemetry: Diagnostic Details Specific Failure Payload External

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Diagnostic details specific to the failure payload or external endpoint MUST be supplied in the extra metadata dictionary rather than concatenated directly into the error message string.

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