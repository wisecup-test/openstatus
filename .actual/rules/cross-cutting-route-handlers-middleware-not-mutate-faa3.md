---
version: 2
name: cross-cutting-route-handlers-middleware-not-mutate-faa3
description: "Hono Request Context Storage for Ephemeral State Propagation: Route Handlers Middleware Not Mutate Global"
metadata:
  actual:
    adr_id: faa3a72e-f78d-424d-8aa8-3b5b8bc33ef3
    status: accepted
    candidate_group_id: 53cdfaf2-4e06-4c56-a2ed-67f5e36d7af3
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Request Context Storage for Ephemeral State Propagation: Route Handlers Middleware Not Mutate Global

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for HTTP server request processing pipelines including middleware functions and route handlers.

### Rules

- **R-HNC-001** MUST_NOT: Route handlers and middleware MUST_NOT mutate global state objects or process-level variables to share data across distinct HTTP transactions.

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
Claude Code MUST NOT skip or defer verification. Pull requests containing direct datastore re-queries for established request metadata or untyped context mutations are rejected.
</enforcement>