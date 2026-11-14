---
version: 2
name: cross-cutting-modules-accessing-runtime-configuration-2463
description: "Next.js Environment Secrets and Configuration Access via process.env: Modules Accessing Runtime Configuration Validate Presence"
metadata:
  actual:
    adr_id: 2463f393-8233-4d08-9dee-7dc63c934d83
    status: accepted
    candidate_group_id: af28e293-b25b-4e4f-9a84-6964ae34513c
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - sentry.edge.config.ts
      - sentry.server.config.ts
      - src/**
---

# Next.js Environment Secrets and Configuration Access via process.env: Modules Accessing Runtime Configuration Validate Presence

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application source code accessing environment configuration, third-party authentication tokens, API secrets, client-exposed identifiers, server API route handlers, proxy boundaries, authentication provider configurations, and client components consuming environment toggles.

### Rules

- **R-ENV-001** SHOULD: Modules accessing runtime configuration SHOULD validate the presence and expected shape of required environment values at system initialization or entry point invocation.
- **R-ENV-002** MUST: Verify that all private authentication tokens and webhook verification secrets reside solely in server execution contexts such as route handlers and server utilities.
- **R-ENV-003** MUST: Ensure that variables required in browser environments adhere strictly to the framework public variable naming convention.

### Verify

```bash
# Discover and run the project static analysis and security scanning tasks to detect exposed secrets or invalid environment variable references.
# Discover and execute the project build and compilation routines to ensure environment variable bundling rules are respected without leakage.
```

**Accept when:**
- Static security scanning completes with zero findings of unencrypted secrets or misplaced credentials.
- Client-side compilation artifacts contain zero references to private server secrets and environment tokens.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>