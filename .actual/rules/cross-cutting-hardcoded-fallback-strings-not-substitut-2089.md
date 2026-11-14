---
version: 2
name: cross-cutting-hardcoded-fallback-strings-not-substitut-2089
description: "Next.js Environment Secrets and Configuration Access via process.env: Hardcoded Fallback Strings Not Substituted Sensitive"
metadata:
  actual:
    adr_id: 20897c75-9b1c-4aff-ac65-5731b2b4c1a3
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

# Next.js Environment Secrets and Configuration Access via process.env: Hardcoded Fallback Strings Not Substituted Sensitive

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application source code accessing environment configuration, third-party authentication tokens, API secrets, client-exposed identifiers, server API route handlers, proxy boundaries, authentication provider configurations, and client components consuming environment toggles.

### Rules

- **R-ENV-001** MUST_NOT: Hardcoded fallback strings MUST NOT be substituted for sensitive credentials or security-critical configuration flags in process.env references.

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