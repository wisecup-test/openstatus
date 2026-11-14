---
version: 2
name: cross-cutting-engineers-discover-ecosystem-lock-artifa-34ba
description: "@sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Engineers Discover Ecosystem Lock Artifact Verify"
metadata:
  actual:
    adr_id: 34ba745b-c17e-447a-b5db-6dafc3d634e4
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

# @sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Engineers Discover Ecosystem Lock Artifact Verify

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** MUST: Engineers MUST discover the ecosystem lock artifact and verify the exact resolved version of @sentry/nextjs before introducing or modifying telemetry integration code.

### Verify

```bash
# Discover and execute the repository build script from the dependency manifest
BUILD_SCRIPT=$(node -p "require('./package.json').scripts?.build ? 'build' : ''")
if [ -n "$BUILD_SCRIPT" ]; then
  npm run $BUILD_SCRIPT
fi

# Discover and run the project test suite
TEST_SCRIPT=$(node -p "require('./package.json').scripts?.test ? 'test' : ''")
if [ -n "$TEST_SCRIPT" ]; then
  npm run $TEST_SCRIPT
fi

# Run static analysis / linter if configured
LINT_SCRIPT=$(node -p "require('./package.json').scripts?.lint ? 'lint' : ''")
if [ -n "$LINT_SCRIPT" ]; then
  npm run $LINT_SCRIPT
fi
```

**Accept when:**
- All application execution targets compile cleanly with telemetry initialization logic included.
- Test suites pass with error boundary and network client error capture assertions satisfied.
- Static analysis checks pass without errors or unauthorized telemetry patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>