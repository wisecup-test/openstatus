---
version: 2
name: cross-cutting-components-executed-client-side-runtime-6ee3
description: "Next.js Runtime Environment Variable Configuration for Secrets Handling: Components Executed Client Side Runtime Environments"
metadata:
  actual:
    adr_id: 6ee3d9e1-44c6-439f-b113-eff5004abc6e
    status: accepted
    candidate_group_id: aeae75d1-c923-499e-bbb1-3d56f8c3f818
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - src/**
---

# Next.js Runtime Environment Variable Configuration for Secrets Handling: Components Executed Client Side Runtime Environments

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background tasks, shared libraries, and client components within the web application requiring environment configuration or operational credentials.

### Rules

- **R-NEXT-001** MUST_NOT: Components executed in client-side runtime environments MUST_NOT reference non-public environment variables on process.env that contain sensitive authentication tokens, database credentials, or secret webhook URLs.

### Verify

```bash
# Discover and execute project code analysis scripts to detect unauthorized hardcoded credentials or invalid env references
# Discover and execute project automated test suites
```

**Accept when:**
- Static code analysis confirms no hardcoded secret strings or private credential tokens are committed in source files.
- Automated test suites pass without referencing undefined critical environment variables.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing hardcoded credentials or leaking server environment variables to client contexts are blocked from merging.
</enforcement>