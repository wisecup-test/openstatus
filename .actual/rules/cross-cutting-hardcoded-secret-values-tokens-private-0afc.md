---
version: 2
name: cross-cutting-hardcoded-secret-values-tokens-private-0afc
description: "Next.js Runtime Environment Variable Configuration for Secrets Handling: Hardcoded Secret Values Tokens Private Endpoint"
metadata:
  actual:
    adr_id: 0afc8b7c-9875-4e91-aba2-a08a257b579a
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

# Next.js Runtime Environment Variable Configuration for Secrets Handling: Hardcoded Secret Values Tokens Private Endpoint

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background tasks, shared libraries, and client components within the web application requiring environment configuration or operational credentials.

### Rules

- **R-NEXT-001** MUST_NOT: Hardcoded secret values, tokens, or private endpoint credentials MUST_NOT be committed into source code.

### Verify

```bash
# Discover and execute the project code analysis scripts to detect unauthorized hardcoded credentials or invalid environment variable references.
# Discover and execute the project automated test suites to confirm route handlers correctly handle configured and unconfigured external service endpoints.
```

**Accept when:**
- Static code analysis confirms no hardcoded secret strings or private credential tokens are committed in source files.
- Automated test suites pass without referencing undefined critical environment variables.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>