---
version: 2
name: cross-cutting-engineers-discover-repository-lock-artif-ee7c
description: "Adoption of @sentry/deno for Workflow Exception Telemetry: Engineers Discover Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: ee7c8fb3-7913-4c0e-b019-4d85836000e6
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

# Adoption of @sentry/deno for Workflow Exception Telemetry: Engineers Discover Repository Lock Artifact Verify

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/lib/**`.

These rules are ALWAYS ACTIVE for all asynchronous workflow runners, background cron tasks, and scheduled job handlers within workflow services.

### Rules

- **R-SENTRY-001** MUST: Engineers MUST discover the repository lock artifact and verify the exact resolved version of the telemetry client prior to introducing or modifying error reporting integrations.

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