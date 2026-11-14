---
version: 2
name: cross-cutting-client-accessible-environment-values-exp-65c3
description: "Next.js Environment Secrets and Configuration Access via process.env: Client Accessible Environment Values Explicitly Use"
metadata:
  actual:
    adr_id: 65c344a1-7bed-4b02-92e2-6f568d92e7f9
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

# Next.js Environment Secrets and Configuration Access via process.env: Client Accessible Environment Values Explicitly Use

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application source code accessing environment configuration, third-party authentication tokens, API secrets, client-exposed identifiers, server API route handlers, proxy boundaries, authentication provider configurations, and client components consuming environment toggles.

### Rules

- **R-NEXT-ENV-001** MUST: Client-accessible environment values MUST explicitly use the framework public prefix convention to allow intentional compilation into client bundles.

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