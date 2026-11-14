---
version: 2
name: cross-cutting-before-introducing-modifying-code-that-ed3f
description: "Next.js Runtime Environment Variable Configuration: Before Introducing Modifying Code That Depends"
metadata:
  actual:
    adr_id: ed3fa85c-99aa-44ad-abe8-bda508de8e21
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

# Next.js Runtime Environment Variable Configuration: Before Introducing Modifying Code That Depends

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background cron jobs, server-rendered components, client instrumentation hooks, and shared protocol utilities requiring runtime environment parameters.

### Rules

- **R-ENV-001** MUST: Before introducing or modifying code that depends on framework runtime configuration features, developers MUST discover the dependency lock artifact in the repository and verify the exact resolved version of the framework.

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