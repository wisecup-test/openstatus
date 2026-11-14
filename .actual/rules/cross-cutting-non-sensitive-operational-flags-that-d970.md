---
version: 2
name: cross-cutting-non-sensitive-operational-flags-that-d970
description: "Next.js Process Environment Secrets and Credentials Handling: Non Sensitive Operational Flags That Control"
metadata:
  actual:
    adr_id: d9706ee5-bc3d-4f91-9b77-12938de01e07
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

# Next.js Process Environment Secrets and Credentials Handling: Non Sensitive Operational Flags That Control

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MAY: Non-sensitive operational flags that control client interface indicators MAY be evaluated in client components provided they use standard public framework configuration patterns.

### Verify

```bash
# Discover and execute the repository's static analysis and linting scripts from workspace configuration
# Discover and execute the project's build and compilation scripts to ensure production bundling succeeds without leaking server-restricted environment symbols
```

**Accept when:**
- Static analysis checks report zero unauthorized process environment accesses in client compilation units.
- Application build and type verification succeed with clear separation between server and client configuration bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>