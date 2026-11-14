---
version: 2
name: cross-cutting-before-implementing-changes-additions-th-4df5
description: "process.env Runtime Configuration Management: Before Implementing Changes Additions That Rely"
metadata:
  actual:
    adr_id: 4df548e1-a3b7-4ffa-94d8-cf02afc47612
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

# process.env Runtime Configuration Management: Before Implementing Changes Additions That Rely

Scope: applies to `apps/status-page/instrumentation-client.ts`, `apps/status-page/next.config.ts`, `apps/status-page/sentry.edge.config.ts`, `apps/status-page/sentry.server.config.ts`, `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all code or configurations relying on runtime dependencies, platform SDKs, or process environment variables across client, edge, and server execution contexts.

### Rules

- **R-PRC-001** MUST: Before implementing changes or additions that rely on external runtime dependencies or platform SDKs, the consumer MUST inspect the repository dependency manifest and authoritative lock artifact to verify the exact resolved versions and their supported environment configuration specifications.

### Verify

```bash
# Discover the repository configuration manifest and execute static analysis/linting for environment variable usage
# Discover and execute project build verification to ensure client bundle compilation does not include unisolated server environment identifiers
# Search source files for direct process environment invocations to verify scoping and security compliance
```

**Accept when:**
- All process environment variable lookups comply with boundary isolation rules between server and client contexts.
- The project static analysis and build verification scripts complete with zero errors related to environment configuration.
- All referenced environment variables have documented fallback values or are verified present in deployment environment definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>