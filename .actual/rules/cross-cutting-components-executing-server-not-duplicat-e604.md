---
version: 2
name: cross-cutting-components-executing-server-not-duplicat-e604
description: "Server-Side @trpc/client Request Header and Cookie Forwarding: Components Executing Server Not Duplicate Custom"
metadata:
  actual:
    adr_id: e604565a-f47d-4cab-b6e2-9ae88bd3aa7a
    status: accepted
    candidate_group_id: db0f7af0-196a-43da-9025-c6af7949ac11
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/trpc/**
---

# Server-Side @trpc/client Request Header and Cookie Forwarding: Components Executing Server Not Duplicate Custom

Scope: applies to `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SRV-001** MUST_NOT: Components executing on the server MUST NOT duplicate custom fetch credential logic outside the centralized server client definition.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>