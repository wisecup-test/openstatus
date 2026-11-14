---
version: 2
name: cross-cutting-application-restrict-direct-access-serve-e05c
description: "Next.js Process Environment Secrets and Credentials Handling: Application Restrict Direct Access Server Only"
metadata:
  actual:
    adr_id: e05c4a20-2a6b-4dc3-86ad-c327df63813d
    status: accepted
    candidate_group_id: 7c7309e0-f7a5-478c-9be6-6a5a8311816d
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# Next.js Process Environment Secrets and Credentials Handling: Application Restrict Direct Access Server Only

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all application modules, route handlers, authentication providers, and interface components accessing environment variables and credentials within the framework.

### Rules

- **R-NEXT-001** MUST: Restrict direct access to server-only credentials and secret tokens to server and edge runtime contexts, strictly preventing reference to non-public environment variables within client-rendered modules.

### Verify

```bash
# Discover and execute the repository's static analysis and linting scripts from workspace configuration
# Discover and execute the project's build and compilation scripts to verify production bundling succeeds without leaking server-restricted environment symbols
```

**Accept when:**
- Static analysis checks report zero unauthorized process environment accesses in client compilation units.
- Application build and type verification succeed with clear separation between server and client configuration bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>