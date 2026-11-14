---
version: 2
name: cross-cutting-environment-variables-containing-critica-fed3
description: "Next.js Runtime Environment Variable Configuration for Secrets Handling: Environment Variables Containing Critical Secrets External"
metadata:
  actual:
    adr_id: fed3f28a-eed1-4324-abb1-ef55d2c0c8e3
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

# Next.js Runtime Environment Variable Configuration for Secrets Handling: Environment Variables Containing Critical Secrets External

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background tasks, shared libraries, and client components within the web application requiring environment configuration or operational credentials.

### Rules

- **R-ENV-001** SHOULD: Environment variables containing critical secrets or external endpoint URLs SHOULD be validated for presence and format at component initialization to prevent runtime failures caused by missing configuration.

### Verify

```bash
# Discover and execute project code analysis and test scripts
npm run lint || pnpm lint || yarn lint
npm test || pnpm test || yarn test
```

**Accept when:**
- Static code analysis confirms no hardcoded secret strings or private credential tokens are committed in source files.
- Automated test suites pass without referencing undefined critical environment variables.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing hardcoded credentials or leaking server environment variables to client contexts are blocked from merging.
</enforcement>