---
version: 2
name: cross-cutting-error-boundaries-remote-procedure-call-eb3a
description: "Exception Capture and Error Tracing via @sentry/nextjs: Error Boundaries Remote Procedure Call Callbacks"
metadata:
  actual:
    adr_id: eb3aa60e-71ca-4869-9af0-89f1fa251ecf
    status: accepted
    candidate_group_id: 0aa494d9-538b-4b0c-a5ed-a28a60aec767
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
      - src/trpc/**
---

# Exception Capture and Error Tracing via @sentry/nextjs: Error Boundaries Remote Procedure Call Callbacks

Scope: applies to `apps/web/src/app/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** MUST_NOT: Error boundaries and remote procedure call error callbacks MUST NOT suppress or swallow uncaught exceptions without recording them through Sentry.captureException.

### Verify

```bash
# Discover dependency manifest and lock artifact, and verify the resolved version of @sentry/nextjs
# (Tool/command discovery per mandatory policy)

# Execute project static analysis and lint command to verify compliance with Sentry.captureException rules
# Run project test suite to verify error boundary and procedure error handler test cases dispatch error tracing calls
```

**Accept when:**
- All global runtime error boundaries and remote procedure call error handlers invoke Sentry.captureException when errors occur.
- Remote procedure call error capture includes structured contextual metadata containing the procedure path.
- Repository verification and static analysis suites pass without unhandled exception reporting violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines and static analysis reject pull requests violating these rules, and build-blocking violations require engineering remediation.
</enforcement>