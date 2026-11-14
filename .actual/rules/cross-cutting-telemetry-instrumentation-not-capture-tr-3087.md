---
version: 2
name: cross-cutting-telemetry-instrumentation-not-capture-tr-3087
description: "Adoption of @sentry/deno for Workflow Exception Telemetry: Telemetry Instrumentation Not Capture Transmit Sensitive"
metadata:
  actual:
    adr_id: 308742c4-da4d-4808-a1e9-7f9487187ad0
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

# Adoption of @sentry/deno for Workflow Exception Telemetry: Telemetry Instrumentation Not Capture Transmit Sensitive

Scope: applies to `apps/workflows/src/cron/**`, `apps/workflows/src/lib/**`.

These rules are ALWAYS ACTIVE for all asynchronous workflow runners, background cron tasks, and scheduled job handlers within workflow services.

### Rules

- **R-TELEMETRY-001** MUST_NOT: Telemetry instrumentation MUST NOT capture or transmit sensitive credentials, authorization secrets, or private environment variables within exception tags or extra payload metadata.

### Verify

```bash
# Discover the project workspace test runner and execute the automated test suite covering workflow error handling and exception capture.
# Discover the project linter and static analysis runner, then execute code validation against workflow telemetry integrations.
```

**Accept when:**
- The automated test suite verifies that error handling routines invoke exception capture with required contextual tags.
- Static analysis and dependency verification confirm that error tracking imports resolve exclusively to the adopted runtime SDK without unverified version discrepancies.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis, automated test suites, and peer code reviews audit error handling boundaries in workflow and cron service modules.
</enforcement>