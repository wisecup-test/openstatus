---
version: 2
name: cross-cutting-route-handlers-client-initializers-consu-5e6e
description: "Next.js Runtime Environment Variable Configuration: Route Handlers Client Initializers Consuming Critical"
metadata:
  actual:
    adr_id: 5e6e4e62-beab-4d5f-8d51-b2d8fa1a2ebb
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

# Next.js Runtime Environment Variable Configuration: Route Handlers Client Initializers Consuming Critical

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background cron jobs, server-rendered components, and client instrumentation hooks requiring runtime environment parameters.

### Rules

- **R-ENV-001** SHOULD: Route handlers and client initializers consuming critical environment variables SHOULD verify the presence of required values prior to initiating network requests.

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