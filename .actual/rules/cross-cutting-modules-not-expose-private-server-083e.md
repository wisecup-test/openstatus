---
version: 2
name: cross-cutting-modules-not-expose-private-server-083e
description: "process.env Runtime Configuration Management: Modules Not Expose Private Server Credentials"
metadata:
  actual:
    adr_id: 083e5d26-3e24-48f3-8297-49aa6e8a510a
    status: accepted
    candidate_group_id: aa264d68-51a8-4cce-a40f-bb3bf9575f06
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

# process.env Runtime Configuration Management: Modules Not Expose Private Server Credentials

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-37-001** MUST_NOT: Modules MUST_NOT expose private server credentials or sensitive operational keys to client-side bundles when configuring environment variables.

### Verify

```bash
# Discover the repository configuration manifest and execute the designated static analysis or linting script to identify unvalidated environment variable usages.
# Discover and execute the project build verification script to ensure client bundle compilation does not include unisolated server environment identifiers.
# Execute a search across source files for direct process environment invocations to verify compliance with scoping and security rules.
```

**Accept when:**
- All process environment variable lookups comply with boundary isolation rules between server and client contexts.
- The project static analysis and build verification scripts complete with zero errors related to environment configuration.
- All referenced environment variables have documented fallback values or are verified present in deployment environment definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>