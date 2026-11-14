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

These rules are ALWAYS ACTIVE for global application error boundary components and shared remote procedure call client error interceptors and links.

### Rules

- **R-SEN-001** MUST: Before implementing or updating any error tracing integration, engineers MUST inspect the repository dependency resolution artifact to determine the exact resolved version of @sentry/nextjs and verify API compatibility against official version-specific documentation.
- **R-SEN-002** MUST: Execute the lock-version grounding process: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists before use, and re-run steps per dependency at point of use for version-sensitive behavior.
- **R-SEN-003** MUST: Configure boundary error capture inside client lifecycle hooks to prevent rendering loops while ensuring uncaught render errors are dispatched.
- **R-SEN-004** MUST: Pass operational metadata such as invocation paths within the extra field rather than serializing sensitive raw payloads.

### Verify

```bash
# Discover the repository script runner and execute the static analysis check for tracing boundary compliance.
# Run the test suite target responsible for validating client error boundary exception dispatching.
```

**Accept when:**
- Static analysis confirms that unhandled boundary error handlers invoke Sentry.captureException.
- Automated verification passes with contextual path metadata validated in remote call error interceptors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks enforcing telemetry dispatch in boundary handlers and peer code review.
</enforcement>