---
version: 2
name: cross-cutting-before-implementing-any-code-that-2b8c
description: "Next.js Runtime Environment Variable Configuration for Secrets Handling: Before Implementing Any Code That Accesses"
metadata:
  actual:
    adr_id: 2b8cc34a-0781-4c72-9b55-25d5261931fd
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

# Next.js Runtime Environment Variable Configuration for Secrets Handling: Before Implementing Any Code That Accesses

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for all server-side route handlers, background tasks, shared libraries, and client components within the web application requiring environment configuration or operational credentials.

### Rules

- **R-NEXT-001** MUST: Before implementing any code that accesses framework environment configuration, the consumer MUST inspect the repository dependency manifest and resolution artifact to resolve the exact locked version of the framework.

### Verify

```bash
# Discover and execute project code analysis scripts to detect unauthorized hardcoded credentials or invalid environment variable references
# Discover and execute project automated test suites to confirm route handlers correctly handle configured and unconfigured external service endpoints
```

**Accept when:**
- Static code analysis confirms no hardcoded secret strings or private credential tokens are committed in source files.
- Automated test suites pass without referencing undefined critical environment variables.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>