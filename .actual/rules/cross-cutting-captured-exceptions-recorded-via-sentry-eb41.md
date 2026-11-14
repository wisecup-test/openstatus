---
version: 2
name: cross-cutting-captured-exceptions-recorded-via-sentry-eb41
description: "Adoption of @sentry/deno for Workflow Exception Telemetry: Captured Exceptions Recorded Via Sentry Captureexception"
metadata:
  actual:
    adr_id: eb413f82-b992-4ac9-8452-eaef03ed9fa2
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

# Adoption of @sentry/deno for Workflow Exception Telemetry: Captured Exceptions Recorded Via Sentry Captureexception

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/lib/**`.

These rules are ALWAYS ACTIVE for asynchronous workflow runners, background cron tasks, and scheduled job handlers within workflow services.

### Rules

- **R-SENTRY-001** MUST: Captured exceptions MUST be recorded via Sentry.captureException and MUST supply contextual operational tags defining the job classification, execution phase, and operational entity identifier.

### Verify

```bash
# Discover and execute the project workspace test runner covering workflow error handling and exception capture
# Discover and execute the project linter and static analysis runner for workflow telemetry integrations
```

**Accept when:**
- The automated test suite verifies that error handling routines invoke exception capture with required contextual tags.
- Static analysis and dependency verification confirm that error tracking imports resolve exclusively to the adopted runtime SDK without unverified version discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by static analysis, type checking, automated test suite coverage, and peer code reviews.
</enforcement>