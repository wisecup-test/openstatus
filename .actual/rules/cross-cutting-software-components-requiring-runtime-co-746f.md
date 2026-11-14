---
version: 2
name: cross-cutting-software-components-requiring-runtime-co-746f
description: "Next.js Runtime Environment Variable Configuration for Secrets Handling: Software Components Requiring Runtime Configuration Secrets"
metadata:
  actual:
    adr_id: 746f144d-45cb-4fe8-a321-c05fdf7e73e7
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

# Next.js Runtime Environment Variable Configuration for Secrets Handling: Software Components Requiring Runtime Configuration Secrets

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background tasks, shared libraries, and client components within the web application requiring environment configuration or operational credentials.

### Rules

- **R-ENV-001** MUST: Software components requiring runtime configuration or secrets MUST access them through the framework runtime environment interface using process.env rather than importing or managing dedicated third-party secrets management client SDKs.

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