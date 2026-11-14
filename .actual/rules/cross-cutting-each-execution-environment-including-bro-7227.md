---
version: 2
name: cross-cutting-each-execution-environment-including-bro-7227
description: "Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Each Execution Environment Including Browser Client"
metadata:
  actual:
    adr_id: 7227d6d9-61dc-4fbc-bc18-9916a608e14e
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

# Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Each Execution Environment Including Browser Client

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/sentry.edge.config.ts`, `apps/web/sentry.server.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all application modules executing within browser, edge, and server runtimes requiring exception tracking and error observability, as well as shared API routing and communication layers handling remote procedure errors and cron triggers.

### Rules

- **R-SENTRY-001** MUST: Each execution environment—including browser client, Node.js server, and edge runtime—MUST initialize error monitoring through dedicated environment configuration modules.

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
Claude Code MUST NOT skip or defer verification. Automated static analysis, build pipelines, and CI checks verify proper initialization and integration of @sentry/nextjs across all entry points, blocking unmonitored deployments or unhandled API error paths.
</enforcement>