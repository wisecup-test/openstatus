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
- **R-TRPC-002** MANDATORY: Construct the server API client transport using a dedicated request-scoped context that extracts existing incoming cookies.
- **R-TRPC-003** MANDATORY: Ensure custom fetch options retain incoming request headers while overriding target credential and cookie parameters.
- **R-TRPC-004** MANDATORY: Execute lock-version grounding before writing code that uses a versioned library, following the ordered discovery and validation steps.
- **R-TRPC-005** MANDATORY: Enforce server-only boundary directives and automated boundary checks to prevent server client code from entering browser bundles.
- **R-TRPC-006** MANDATORY: Audit server-side fetch caching configurations and ensure dynamic request headers force per-request resolution where user data is returned.

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
Claude Code MUST NOT skip or defer verification. Verified by automated architectural boundary linting, continuous integration test suites, and mandatory peer code reviews. Violations will be blocked during automated verification or must be refactored before merging.
</enforcement>