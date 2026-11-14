---
version: 2
name: cross-cutting-modules-providing-server-side-clients-d6a3
description: "Server-Side @trpc/client Request Header and Cookie Forwarding: Modules Providing Server Side Clients Enforce"
metadata:
  actual:
    adr_id: d6a3fbbb-8049-476c-b9f3-ca754923cc79
    status: accepted
    candidate_group_id: db0f7af0-196a-43da-9025-c6af7949ac11
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/trpc/**
---

# Server-Side @trpc/client Request Header and Cookie Forwarding: Modules Providing Server Side Clients Enforce

Scope: applies to `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Modules providing server-side API clients MUST enforce server-only execution boundaries to prevent accidental inclusion in client-side bundles.

### Verify

```bash
# Discover and execute the project verification script for static boundary and lint enforcement.
# Discover and run the project automated test suite covering server-side data fetching and internal API transport.
```

**Accept when:**
- Internal API calls executed within server-side environments successfully convey authenticated session headers without manual per-call header construction.
- Static boundary checks prevent client-side evaluation of server-scoped API client configurations.
- Automated test suites simulating server-rendered data fetching execute without authentication transport errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>