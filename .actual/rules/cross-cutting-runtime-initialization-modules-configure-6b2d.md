---
version: 2
name: cross-cutting-runtime-initialization-modules-configure-6b2d
description: "@sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Runtime Initialization Modules Configure Separate Instrumentation"
metadata:
  actual:
    adr_id: 6b2dbf7f-43b4-4dcd-84f3-bd567e24e917
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

# @sentry/nextjs Adoption for Cross-Runtime Error Tracking and Observability: Runtime Initialization Modules Configure Separate Instrumentation

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application user interface error boundaries, execution initialization workflows, and remote procedure call client middleware.

### Rules

- **R-INIT-001** MUST: Runtime initialization modules MUST configure separate instrumentation handlers for client, server, and edge runtime targets to isolate environment-specific lifecycles.

### Verify

```bash
# Discover and run the build script from the dependency manifest to verify compilation
build_script=$(node -p "require('./package.json').scripts.build || 'npm run build'")
eval $build_script

# Discover and run the test suite
test_script=$(node -p "require('./package.json').scripts.test || 'npm test'")
eval $test_script

# Discover and run the linter/static analysis
lint_script=$(node -p "require('./package.json').scripts.lint || 'npm run lint'")
eval $lint_script
```

**Accept when:**
- All application execution targets compile cleanly with telemetry initialization logic included.
- Test suites pass with error boundary and network client error capture assertions satisfied.
- Static analysis checks pass without errors or unauthorized telemetry patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>