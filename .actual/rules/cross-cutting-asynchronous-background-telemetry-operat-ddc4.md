---
version: 2
name: cross-cutting-asynchronous-background-telemetry-operat-ddc4
description: "Evaluation of after Callback within @trpc/server Middleware: Asynchronous Background Telemetry Operations Scheduled Within"
metadata:
  actual:
    adr_id: ddc4a9c6-2f0f-458e-9232-85d8417a6934
    status: accepted
    candidate_group_id: 488a7432-7d16-4be8-9ce8-5827c5960bf3
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Evaluation of after Callback within @trpc/server Middleware: Asynchronous Background Telemetry Operations Scheduled Within

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for server-side procedure middleware pipelines defined with @trpc/server.

### Rules

- **R-TRPC-001** SHOULD: Asynchronous background telemetry operations scheduled within @trpc/server middleware procedures SHOULD be validated through dedicated test suites rather than procedure-level execution hooks.

### Verify

```bash
# Discover and run the project test execution script from the repository manifest to verify procedure logic independently.
npm test
```

**Accept when:**
- The project test verification scripts complete without relying on procedure middleware lifecycle hooks for test assertions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review during pull requests and automated continuous integration pipeline checks enforce clean separation of runtime middleware and test suites.
</enforcement>