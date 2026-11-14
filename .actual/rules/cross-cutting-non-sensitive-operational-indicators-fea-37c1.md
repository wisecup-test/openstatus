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

These rules are ALWAYS ACTIVE for all code handling runtime configuration, environment variables, operational indicators, and feature flags across presentation, edge, and server execution contexts.

### Rules

- **R-37-001** MAY: Non-sensitive operational indicators and feature flags MAY be accessed directly within presentation components when conditional rendering depends on the active deployment environment.

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
Claude Code MUST NOT skip or defer verification. All process environment accesses must be audited for boundary isolation, and non-compliant direct lookups or unisolated credential references must be blocked or refactored.
</enforcement>