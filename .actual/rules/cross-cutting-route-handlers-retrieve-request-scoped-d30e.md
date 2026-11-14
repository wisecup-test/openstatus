---
version: 2
name: cross-cutting-route-handlers-retrieve-request-scoped-d30e
description: "Hono Request Context Storage for Ephemeral State Propagation: Route Handlers Retrieve Request Scoped Tenant"
metadata:
  actual:
    adr_id: d30e970a-4c91-444c-8aec-6cc410d58b3b
    status: accepted
    candidate_group_id: 53cdfaf2-4e06-4c56-a2ed-67f5e36d7af3
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Request Context Storage for Ephemeral State Propagation: Route Handlers Retrieve Request Scoped Tenant

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for HTTP server request processing pipelines including middleware functions and route handlers.

### Rules

- **R-HNC-001** MUST: Route handlers MUST retrieve request-scoped tenant and authentication metadata through context getter methods instead of re-querying primary datastores.

### Verify

```bash
# Discover and execute the repository type-checking script from the project manifest
# Discover and execute the repository static analysis and linting scripts
# Discover and run the project automated test suite
```

**Accept when:**
- All route handlers retrieve pre-authenticated tenant and telemetry metadata via context getter methods without re-querying datastores.
- Context variable keys and values conform to static type definitions verified by repository validation checks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>