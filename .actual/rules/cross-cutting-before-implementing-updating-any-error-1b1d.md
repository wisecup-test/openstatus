---
version: 2
name: cross-cutting-before-implementing-updating-any-error-1b1d
description: "@sentry/nextjs Exception Capture and Boundary Tracing: Before Implementing Updating Any Error Tracing"
metadata:
  actual:
    adr_id: 1b1d6cf2-f609-4529-850b-794e7385314d
    status: accepted
    candidate_group_id: a868f887-5323-4bac-b14e-89765e49358b
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/lib/trpc/**
---

# @sentry/nextjs Exception Capture and Boundary Tracing: Before Implementing Updating Any Error Tracing

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** MUST: Before implementing or updating any error tracing integration, inspect the repository dependency resolution artifact to determine the exact resolved version of @sentry/nextjs and verify API compatibility against official version-specific documentation.

### Verify

```bash
# Discover the repository script runner and execute the static analysis check for tracing boundary compliance
# Run the test suite target responsible for validating client error boundary exception dispatching
```

**Accept when:**
- Static analysis confirms that unhandled boundary error handlers invoke Sentry.captureException.
- Automated verification passes with contextual path metadata validated in remote call error interceptors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>