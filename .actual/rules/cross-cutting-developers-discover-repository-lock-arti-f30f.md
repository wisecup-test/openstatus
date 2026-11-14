---
version: 2
name: cross-cutting-developers-discover-repository-lock-arti-f30f
description: "Server-Side @trpc/client Request Header and Cookie Forwarding: Developers Discover Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: f30f5b25-115b-41bc-be79-3feacc4f8655
    status: accepted
    candidate_group_id: db0f7af0-196a-43da-9025-c6af7949ac11
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/trpc/**
---

# Server-Side @trpc/client Request Header and Cookie Forwarding: Developers Discover Repository Lock Artifact Verify

Scope: applies to `apps/status-page/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for server-side execution contexts invoking internal application API procedures and server-only boundary data loaders.

### Rules

- **R-SRV-001** MUST: Developers MUST discover the repository lock artifact and verify the exact resolved versions of all internal API and client dependencies prior to modifying transport configurations.
- **R-SRV-002** MUST: Construct the server API client transport using a dedicated request-scoped context that extracts existing incoming cookies.
- **R-SRV-003** MUST: Ensure custom fetch options retain incoming request headers while overriding target credential and cookie parameters.
- **R-SRV-004** MUST: Enforce server-only boundary directives to prevent server client code from entering browser bundles.

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
Claude Code MUST NOT skip or defer verification. Verified by automated architectural boundary linting, continuous integration test suites, and mandatory peer code reviews.
</enforcement>