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

- **R-ENV-001** MUST: Prior to implementing or updating configuration access patterns involving versioned framework dependencies, the consumer MUST inspect the workspace dependency lock artifact and resolve the exact locked version against official documentation.
- **R-ENV-002** MUST: Maintain strict runtime boundary separation for process environment reads, ensuring server secrets remain excluded from client compilation artifacts.
- **R-ENV-003** MUST: Execute lock-version grounding in order before writing code that uses a versioned library: find dependency manifest, identify build tool, inspect repository lock/resolution artifact, look up official documentation for that exact version, confirm API/class/function existence, and re-run per dependency at point of use for version-sensitive behavior.

### Verify

```bash
# Discover and execute the project's static analysis and linting scripts
# Example discovery:
npm run lint || yarn lint || pnpm lint

# Discover and execute the project's build and compilation scripts
npm run build || yarn build || pnpm build
```

**Accept when:**
- Static analysis checks report zero unauthorized process environment accesses in client compilation units.
- Application build and type verification succeed with clear separation between server and client configuration bindings.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>