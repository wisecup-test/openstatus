---
version: 2
name: cross-cutting-modules-accessing-environment-configurat-2772
description: "process.env Runtime Configuration Management: Modules Accessing Environment Configuration Variables Validate"
metadata:
  actual:
    adr_id: 277243fc-a474-48ff-be3c-bb09dbda9f3a
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

# process.env Runtime Configuration Management: Modules Accessing Environment Configuration Variables Validate

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for modules accessing environment configuration variables across client, edge, and server execution contexts.

### Rules

- **R-ENV-001** SHOULD: Modules accessing environment configuration variables SHOULD validate the presence and format of required variables at module load or initialization time, handling missing values gracefully.

### Verify

```bash
# Discover the repository configuration manifest and execute the designated static analysis or linting script
# Discover and execute the project build verification script to ensure client bundle compilation does not include unisolated server environment identifiers
# Execute a search across source files for direct process environment invocations to verify compliance with scoping and security rules
```

**Accept when:**
- All process environment variable lookups comply with boundary isolation rules between server and client contexts.
- The project static analysis and build verification scripts complete with zero errors related to environment configuration.
- All referenced environment variables have documented fallback values or are verified present in deployment environment definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>