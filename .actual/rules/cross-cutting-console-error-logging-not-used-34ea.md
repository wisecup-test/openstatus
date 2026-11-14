---
version: 2
name: cross-cutting-console-error-logging-not-used-34ea
description: "Adoption of @sentry/deno for Workflow Exception Telemetry: Console Error Logging Not Used Replacement"
metadata:
  actual:
    adr_id: 34ead4c2-b053-42f9-b124-3b15b2055224
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

# Adoption of @sentry/deno for Workflow Exception Telemetry: Console Error Logging Not Used Replacement

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/lib/**`.

These rules are ALWAYS ACTIVE for asynchronous workflow runners, background cron tasks, and scheduled job handlers within workflow services.

### Rules

- **R-SENTRY-001** SHOULD_NOT: Console error logging SHOULD NOT be used as a replacement for structured exception dispatch via the adopted error tracking client.

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