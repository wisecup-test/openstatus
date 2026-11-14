---
version: 2
name: cross-cutting-remote-procedure-call-error-interceptors-797d
description: "@sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Remote Procedure Call Error Interceptors Invoke"
metadata:
  actual:
    adr_id: 797d83cf-5a44-45d7-ae42-d3b3e3811ac3
    status: accepted
    candidate_group_id: 002e37f3-4d0a-434f-ae9a-10f3bc6c6348
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# @sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Remote Procedure Call Error Interceptors Invoke

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application user interface error boundaries, fallback handlers, client/server/edge execution initialization workflows, and remote procedure call client middleware and error handling handlers.

### Rules

- **R-RPC-001** MUST: Remote procedure call error interceptors MUST invoke Sentry.captureException with associated operation path metadata when request execution fails.

### Verify

```bash
# Discover and execute compilation pipeline to verify telemetry instrumentation compiles cleanly
BUILD_SCRIPT=$(node -p "require('./package.json').scripts?.build ? 'build' : ''")
if [ -n "$BUILD_SCRIPT" ]; then
  npm run build
fi

# Discover and run test suite
test_script=$(node -p "require('./package.json').scripts?.test ? 'test' : ''")
if [ -n "$test_script" ]; then
  npm test
fi

# Discover and run linter/static analysis
lint_script=$(node -p "require('./package.json').scripts?.lint ? 'lint' : ''")
if [ -n "$lint_script" ]; then
  npm run lint
fi
```

**Accept when:**
- All application execution targets compile cleanly with telemetry initialization logic included.
- Test suites pass with error boundary and network client error capture assertions satisfied.
- Static analysis checks pass without errors or unauthorized telemetry patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>