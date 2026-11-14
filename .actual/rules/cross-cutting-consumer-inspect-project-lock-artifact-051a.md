---
version: 2
name: cross-cutting-consumer-inspect-project-lock-artifact-051a
description: "Next.js Environment Secrets and Configuration Access via process.env: Consumer Inspect Project Lock Artifact Determine"
metadata:
  actual:
    adr_id: 051a8ddd-1ad6-437f-a889-54ade942652c
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

# Next.js Environment Secrets and Configuration Access via process.env: Consumer Inspect Project Lock Artifact Determine

Scope: applies to `apps/dashboard/instrumentation-client.ts`, `apps/dashboard/next.config.ts`, `apps/dashboard/sentry.edge.config.ts`, `apps/dashboard/sentry.server.config.ts`, `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all application source code accessing environment configuration, third-party authentication tokens, API secrets, and client-exposed identifiers.

### Rules

- **R-ENV-001** MUST: The consumer MUST inspect the project lock artifact to determine the authoritative locked version of the framework dependency before implementing configuration access patterns.
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