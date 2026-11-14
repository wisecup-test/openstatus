---
version: 2
name: cross-cutting-environment-variables-intended-consumpti-726a
description: "Next.js Runtime Environment Variable Configuration: Environment Variables Intended Consumption Within Browser"
metadata:
  actual:
    adr_id: 726a4d4e-98fd-4486-b983-7e6fb6af2364
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

# Next.js Runtime Environment Variable Configuration: Environment Variables Intended Consumption Within Browser

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background cron jobs, server-rendered components, client instrumentation hooks, and shared protocol utilities within the web application.

### Rules

- **R-NEXT-001** MUST: Environment variables intended for consumption within browser contexts MUST adhere to the framework designated public prefix convention.

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