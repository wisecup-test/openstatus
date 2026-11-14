---
version: 2
name: cross-cutting-server-client-transport-preserve-incomin-c20f
description: "Server-Side @trpc/client Request Header and Cookie Forwarding: Server Client Transport Preserve Incoming Request"
metadata:
  actual:
    adr_id: c20f5be6-0d33-4403-9286-0606cea828eb
    status: accepted
    candidate_group_id: db0f7af0-196a-43da-9025-c6af7949ac11
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/trpc/**
---

# Server-Side @trpc/client Request Header and Cookie Forwarding: Server Client Transport Preserve Incoming Request

Scope: applies to `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for server-side execution contexts invoking internal application API procedures and server-only components requiring authenticated session propagation.

### Rules

- **R-TRPC-001** SHOULD: The server client transport preserve incoming request headers when injecting session cookie strings.

### Verify

```bash
# Discover and execute the project verification script for static boundary and lint enforcement
# Discover and run the project automated test suite covering server-side data fetching and internal API transport
```

**Accept when:**
- Internal API calls executed within server-side environments successfully convey authenticated session headers without manual per-call header construction.
- Static boundary checks prevent client-side evaluation of server-scoped API client configurations.
- Automated test suites simulating server-rendered data fetching execute without authentication transport errors.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>