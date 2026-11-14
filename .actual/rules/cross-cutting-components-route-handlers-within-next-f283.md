---
version: 2
name: cross-cutting-components-route-handlers-within-next-f283
description: "Next.js Runtime Environment Variable Configuration: Components Route Handlers Within Next Application"
metadata:
  actual:
    adr_id: f2832cdc-3cf7-43e7-a1cd-f2179acffc0a
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

# Next.js Runtime Environment Variable Configuration: Components Route Handlers Within Next Application

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background cron jobs, server-rendered components, and client instrumentation hooks within the web application.

### Rules

- **R-NEXT-001** MUST: Components and route handlers within the Next.js application MUST consume runtime environment configuration through the runtime process environment interface.

### Verify

```bash
echo "Discover project verification scripts from repository manifest and execute environment check commands"
echo "Inspect codebase for direct process environment variable references across module boundaries"
```

**Accept when:**
- All server route handlers, background tasks, and shared client-server modules resolve required runtime configuration properties without unhandled runtime exceptions
- Client-side bundles remain completely free of server-only secrets and private credential references

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>