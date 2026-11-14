---
version: 2
name: cross-cutting-boundary-error-handlers-not-log-07ba
description: "@sentry/nextjs Exception Capture and Boundary Tracing: Boundary Error Handlers Not Log Errors"
metadata:
  actual:
    adr_id: 07bab3d2-d2b7-4d21-93af-33a2181b738c
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

# @sentry/nextjs Exception Capture and Boundary Tracing: Boundary Error Handlers Not Log Errors

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for global application error boundary components and shared remote procedure call client error interceptors and links.

### Rules

- **R-BOUND-001** MUST_NOT: Boundary error handlers MUST NOT log errors to standard console streams without concurrently capturing the exception via Sentry.captureException.

### Verify

```bash
# Discover the repository script runner and execute the static analysis check for tracing boundary compliance.
# Run the test suite target responsible for validating client error boundary exception dispatching.
```

**Accept when:**
- Static analysis confirms that unhandled boundary error handlers invoke Sentry.captureException.
- Automated verification passes with contextual path metadata validated in remote call error interceptors.

<enforcement>
Automated build checks fail if designated global error boundaries or client links catch exceptions without invoking telemetry capture. Claude Code MUST NOT skip or defer verification.
</enforcement>