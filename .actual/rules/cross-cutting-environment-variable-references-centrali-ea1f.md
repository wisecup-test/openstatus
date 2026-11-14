---
version: 2
name: cross-cutting-environment-variable-references-centrali-ea1f
description: "process.env Runtime Configuration Management: Environment Variable References Centralized Within Domain"
metadata:
  actual:
    adr_id: ea1fb57c-3cc1-4663-b56b-01952ae190af
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

# process.env Runtime Configuration Management: Environment Variable References Centralized Within Domain

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: Environment variable references SHOULD be centralized within domain-specific configuration modules rather than repeated as raw string lookups across presentation components.

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