---
version: 2
name: cross-cutting-developers-discover-project-dependency-l-5bee
description: "Exception Capture and Error Tracing via @sentry/nextjs: Developers Discover Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 5bee607a-92fa-4aa8-8d66-7b4af2caf8ea
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

# Exception Capture and Error Tracing via @sentry/nextjs: Developers Discover Project Dependency Lock Artifact

Scope: applies to `apps/web/src/app/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all client-side global error boundaries and remote procedure call client communication error handling callbacks and link configurations.

### Rules

- **R-SENTRY-001** MUST: Developers MUST discover the project dependency lock artifact and verify the exact resolved version of @sentry/nextjs before introducing or modifying exception capture calls.
- **R-SENTRY-002** MUST: Invoke Sentry.captureException directly inside component lifecycle hooks or effect handlers dedicated to uncaught error capture.
- **R-SENTRY-003** MUST: Pass structured metadata containing the target procedure path within the extra context argument when handling remote procedure call client failures.
- **R-SENTRY-004** MUST: Ensure sensitive authentication credentials or personally identifiable information are omitted or scrubbed prior to dispatching exception metadata.

### Verify

```bash
# Discover the repository dependency manifest and lock artifact, and verify the resolved version of @sentry/nextjs.
# Discover and execute the project static analysis and lint command to verify compliance with Sentry.captureException rules across error boundaries and RPC clients.
# Discover and run the project test suite to verify that error boundary and procedure error handler test cases successfully dispatch error tracing calls.
```

**Accept when:**
- All global runtime error boundaries and remote procedure call error handlers invoke Sentry.captureException when errors occur.
- Remote procedure call error capture includes structured contextual metadata containing the procedure path.
- Repository verification and static analysis suites pass without unhandled exception reporting violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>