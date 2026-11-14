---
version: 2
name: cross-cutting-application-use-sentry-nextjs-standard-15e7
description: "Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Application Use Sentry Nextjs Standard Library"
metadata:
  actual:
    adr_id: 15e79f16-c5f2-4200-b408-a1a95b8850d4
    status: accepted
    candidate_group_id: e73b9b92-e034-4340-9b15-8297af91da59
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Application Use Sentry Nextjs Standard Library

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/sentry.edge.config.ts`, `apps/web/sentry.server.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SENTRY-001** MUST: The application MUST use @sentry/nextjs as the standard library for capturing runtime exceptions, application errors, and tracing telemetry across client, server, and edge environments.

### Verify

```bash
# Discover and execute project validation suites to ensure error tracking configuration validity
npm test || pnpm test || yarn test || true
# Inspect repository manifests and lockfiles to confirm resolved library version consistency
grep -E "@sentry/nextjs" package.json */package.json 2>/dev/null || true
```

**Accept when:**
- All application runtime tiers initialize error tracking via @sentry/nextjs configuration modules without build or runtime exceptions.
- Unhandled exceptions in UI boundaries and API procedures invoke Sentry.captureException with associated diagnostic metadata.
- Lock-version verification verifies that the resolved version of @sentry/nextjs matches repository constraints and approved documentation.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>