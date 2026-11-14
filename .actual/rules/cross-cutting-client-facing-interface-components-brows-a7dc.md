---
version: 2
name: cross-cutting-client-facing-interface-components-brows-a7dc
description: "Next.js Process Environment Secrets and Credentials Handling: Client Facing Interface Components Browser Execution"
metadata:
  actual:
    adr_id: a7dc6aa1-f786-4db5-b021-a1d5a9755540
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

# Next.js Process Environment Secrets and Credentials Handling: Client Facing Interface Components Browser Execution

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all application modules, route handlers, authentication providers, and interface components accessing environment variables and credentials within the framework.

### Rules

- **R-NEXT-001** MUST_NOT: Client-facing interface components and browser execution contexts MUST NOT access environment keys lacking the designated framework public prefix.

### Verify

```bash
# Discover and execute the project's static analysis, linting, build, and compilation scripts
# Example discovery pattern:
# npm run lint
# npm run build
```

**Accept when:**
- Static analysis checks report zero unauthorized process environment accesses in client compilation units.
- Application build and type verification succeed with clear separation between server and client configuration bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests and code modifications touching credential handling or environment variable consumption must pass automated static analysis checks and peer code review.
</enforcement>