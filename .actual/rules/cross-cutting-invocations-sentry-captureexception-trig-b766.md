---
version: 2
name: cross-cutting-invocations-sentry-captureexception-trig-b766
description: "@sentry/nextjs Exception Capture and Boundary Tracing: Invocations Sentry Captureexception Triggered Remote Procedure"
metadata:
  actual:
    adr_id: b7667f43-0bfe-471b-9c60-455254fcc654
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

# @sentry/nextjs Exception Capture and Boundary Tracing: Invocations Sentry Captureexception Triggered Remote Procedure

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** SHOULD: Invocations of Sentry.captureException triggered by remote procedure call failures MUST attach contextual execution metadata including the request pathway under the structured extra context attribute.

### Verify

```bash
# Discover the repository script runner and execute the static analysis check for tracing boundary compliance.
# Run the test suite target responsible for validating client error boundary exception dispatching.
```

**Accept when:**
- Static analysis confirms that unhandled boundary error handlers invoke Sentry.captureException.
- Automated verification passes with contextual path metadata validated in remote call error interceptors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated build checks fail if designated global error boundaries or client links catch exceptions without invoking telemetry capture.
</enforcement>