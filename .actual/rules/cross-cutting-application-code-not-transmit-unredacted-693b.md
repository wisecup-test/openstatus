---
version: 2
name: cross-cutting-application-code-not-transmit-unredacted-693b
description: "Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Application Code Not Transmit Unredacted Secrets"
metadata:
  actual:
    adr_id: 693be85d-97df-435a-a094-3a8303c3afed
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

# Adoption of @sentry/nextjs for Full-Stack Exception Tracking and Error Monitoring: Application Code Not Transmit Unredacted Secrets

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/sentry.edge.config.ts`, `apps/web/sentry.server.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all application modules executing within browser, edge, and server runtimes requiring exception tracking and error observability, and shared API routing and communication layers handling remote procedure errors and cron triggers.

### Rules

- **R-SENTRY-001** MUST_NOT: Application code MUST NOT transmit unredacted secrets, credentials, or sensitive authentication tokens within exception context payloads.

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