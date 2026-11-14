---
version: 2
name: cross-cutting-prior-implementing-updating-configuratio-b555
description: "Next.js Process Environment Secrets and Credentials Handling: Prior Implementing Updating Configuration Access Patterns"
metadata:
  actual:
    adr_id: b5556c1a-0383-440a-8825-cf0dec17aa5e
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

# Next.js Process Environment Secrets and Credentials Handling: Prior Implementing Updating Configuration Access Patterns

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all application modules, route handlers, authentication providers, and interface components accessing environment variables and credentials within the framework.

### Rules

- **R-NXT-001** MUST: Prior to implementing or updating configuration access patterns involving versioned framework dependencies, the consumer MUST inspect the workspace dependency lock artifact and resolve the exact locked version against official documentation.

### Verify

```bash
# Discover and execute the repository's static analysis and linting scripts from workspace configuration
npm run lint || pnpm lint || yarn lint
# Discover and execute the project's build and compilation scripts to verify production bundling
npm run build || pnpm build || yarn build
```

**Accept when:**
- Static analysis checks report zero unauthorized process environment accesses in client compilation units.
- Application build and type verification succeed with clear separation between server and client configuration bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>