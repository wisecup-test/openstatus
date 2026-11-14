---
version: 2
name: cross-cutting-non-sensitive-operational-indicators-fea-37c1
description: "process.env Runtime Configuration Management: Non Sensitive Operational Indicators Feature Flags"
metadata:
  actual:
    adr_id: 37c10564-bd26-439e-9c05-30bded368a81
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

# process.env Runtime Configuration Management: Non Sensitive Operational Indicators Feature Flags

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all configuration, presentation, middleware, and telemetry code involving runtime environment variables.

### Rules

- **R-ENV-001** MAY: Non-sensitive operational indicators and feature flags MAY be accessed directly within presentation components when conditional rendering depends on the active deployment environment.
- **R-ENV-002** MANDATORY: Audit all environment variable references to ensure private credentials remain isolated to server and edge execution boundaries.
- **R-ENV-003** MANDATORY: Group related environment variable accesses into domain-focused helper utilities to prevent duplicated property lookup logic across presentation modules.

### Verify

```bash
# Discover and execute the project static analysis or linting script to identify unvalidated environment variable usages.
# Discover and execute the project build verification script to ensure client bundle compilation does not include unisolated server environment identifiers.
# Execute a search across source files for direct process environment invocations to verify compliance with scoping and security rules.
```

**Accept when:**
- All process environment variable lookups comply with boundary isolation rules between server and client contexts.
- The project static analysis and build verification scripts complete with zero errors related to environment configuration.
- All referenced environment variables have documented fallback values or are verified present in deployment environment definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, bundle verification checks, and peer code review enforce boundary isolation and prevent unverified environment variable usage.
</enforcement>