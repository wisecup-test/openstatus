---
version: 2
name: cross-cutting-user-interface-error-boundary-components-4e0b
description: "@sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: User Interface Error Boundary Components Invoke"
metadata:
  actual:
    adr_id: 4e0ba209-a7b7-4e5f-b51d-b3d697270c4b
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

# @sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: User Interface Error Boundary Components Invoke

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for application user interface error boundaries, fallback handlers, client/server/edge initialization workflows, and remote procedure call client middleware.

### Rules

- **R-SENTRY-001** MUST: User interface error boundary components MUST invoke Sentry.captureException to transmit unhandled rendering failures to the centralized monitoring platform.

### Verify

```bash
# Discover and run the project build script to verify compilation across runtime targets
build_script=$(node -p "require('./package.json').scripts.build || ''")
if [ -n "$build_script" ]; then
  npm run build
fi

# Discover and run the project test suite to verify error boundary exception capture assertions
test_script=$(node -p "require('./package.json').scripts.test || ''")
if [ -n "$test_script" ]; then
  npm test
fi

# Run the repository linter to verify compliance with telemetry conventions and absence of direct console logging
lint_script=$(node -p "require('./package.json').scripts.lint || ''")
if [ -n "$lint_script" ]; then
  npm run lint
fi
```

**Accept when:**
- All application execution targets compile cleanly with telemetry initialization logic included.
- Test suites pass with error boundary and network client error capture assertions satisfied.
- Static analysis checks pass without errors or unauthorized telemetry patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests modifying error boundaries or network clients must include required error capture calls and pass automated linting and test suite executions.
</enforcement>