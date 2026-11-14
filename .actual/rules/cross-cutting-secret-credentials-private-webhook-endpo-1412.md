---
version: 2
name: cross-cutting-secret-credentials-private-webhook-endpo-1412
description: "Next.js Runtime Environment Variable Configuration: Secret Credentials Private Webhook Endpoints Authorization"
metadata:
  actual:
    adr_id: 1412ef1e-4e2e-48a1-8eda-15afece1ffc5
    status: accepted
    candidate_group_id: 01a7d574-6a6a-490d-a13f-d7c47d691d1a
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - src/**
---

# Next.js Runtime Environment Variable Configuration: Secret Credentials Private Webhook Endpoints Authorization

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background cron jobs, server-rendered components, client instrumentation hooks, and shared protocol utilities.

### Rules

- **R-ENV-001** MUST_NOT: Secret credentials, private webhook endpoints, and authorization tokens accessed via the runtime process environment MUST NOT be referenced within client-rendered components or client-facing bundle entry points.

### Verify

```bash
# Discover project verification scripts from repository manifest and execute environment check commands
# Inspect codebase for direct process environment variable references across module boundaries
```

**Accept when:**
- All server route handlers, background tasks, and shared client-server modules resolve required runtime configuration properties without unhandled runtime exceptions
- Client-side bundles remain completely free of server-only secrets and private credential references

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>