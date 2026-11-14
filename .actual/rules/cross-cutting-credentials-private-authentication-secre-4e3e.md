---
version: 2
name: cross-cutting-credentials-private-authentication-secre-4e3e
description: "Next.js Environment Secrets and Configuration Access via process.env: Credentials Private Authentication Secrets Not Use"
metadata:
  actual:
    adr_id: 4e3ece50-acd1-40a2-83e9-f46b6e352bf0
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

# Next.js Environment Secrets and Configuration Access via process.env: Credentials Private Authentication Secrets Not Use

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application source code accessing environment configuration, third-party authentication tokens, API secrets, and client-exposed identifiers.

### Rules

- **R-ENV-001** MUST_NOT: Credentials and private authentication secrets MUST NOT use framework public prefix conventions or be referenced in components marked for client rendering.

### Verify

```bash
# Discover and run the project static analysis and security scanning tasks
# Discover and execute the project build and compilation routines
```

**Accept when:**
- Static security scanning completes with zero findings of unencrypted secrets or misplaced credentials.
- Client-side compilation artifacts contain zero references to private server secrets and environment tokens.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>