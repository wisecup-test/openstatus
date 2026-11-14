---
version: 2
name: cross-cutting-before-implementing-updating-any-code-e245
description: "Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Before Implementing Updating Any Code Referencing"
metadata:
  actual:
    adr_id: e2455716-5a0c-4cbe-bd7c-0ee956e7b6b8
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

# Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Before Implementing Updating Any Code Referencing

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/sentry.edge.config.ts`, `apps/web/sentry.server.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all application modules executing within browser, edge, and server runtimes requiring exception tracking and error observability, and shared API routing and communication layers handling remote procedure errors and cron triggers.

### Rules

- **R-SENTRY-001** MUST: Before implementing or updating any code referencing @sentry/nextjs, the consumer MUST inspect the repository lock artifact, resolve the exact locked version, and verify all invoked APIs against that version documentation.

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
Claude Code MUST NOT skip or defer verification. Automated static analysis and code review checks confirm proper import and initialization of @sentry/nextjs across entry points, and CI pipelines fail if runtime entry points lack required error monitoring configurations.
</enforcement>