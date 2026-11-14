---
version: 2
name: cross-cutting-server-side-invocations-internal-use-96d4
description: "Server-Side @trpc/client Request Header and Cookie Forwarding: Server Side Invocations Internal Use Isolated"
metadata:
  actual:
    adr_id: 96d405bd-6d12-47b2-ae3c-b77923dac6e2
    status: accepted
    candidate_group_id: db0f7af0-196a-43da-9025-c6af7949ac11
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/trpc/**
---

# Server-Side @trpc/client Request Header and Cookie Forwarding: Server Side Invocations Internal Use Isolated

Scope: applies to `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for server-side execution contexts invoking internal application API procedures and server-only components requiring authenticated session propagation.

### Rules

- **R-SS-001** MUST: Server-side invocations of internal APIs MUST use an isolated @trpc/client configured with custom fetch options that forward incoming request cookie headers and set credentials to include.

### Verify

```bash
# Discover and execute project verification script for static boundary and lint enforcement
# Discover and run automated test suite covering server-side data fetching and internal API transport
```

**Accept when:**
- Internal API calls executed within server-side environments successfully convey authenticated session headers without manual per-call header construction.
- Static boundary checks prevent client-side evaluation of server-scoped API client configurations.
- Automated test suites simulating server-rendered data fetching execute without authentication transport errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated architectural boundary linting, continuous integration test suites, and mandatory peer code review.
</enforcement>