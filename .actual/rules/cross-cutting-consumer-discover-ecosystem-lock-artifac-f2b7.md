---
version: 2
name: cross-cutting-consumer-discover-ecosystem-lock-artifac-f2b7
description: "Hono Request Context Storage for Ephemeral State Propagation: Consumer Discover Ecosystem Lock Artifact Verify"
metadata:
  actual:
    adr_id: f2b7ef69-1999-4c67-997d-a984a6179f1c
    status: accepted
    candidate_group_id: 53cdfaf2-4e06-4c56-a2ed-67f5e36d7af3
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Request Context Storage for Ephemeral State Propagation: Consumer Discover Ecosystem Lock Artifact Verify

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: The consumer MUST discover the ecosystem lock artifact and verify the exact resolved version of the HTTP routing framework before implementation.
- **R-HONO-002** MUST: Define a centralized context variable interface when initializing the HTTP application to provide type safety across all setter and getter invocations.
- **R-HONO-003** MUST: Sequence authentication and tracing middlewares at the head of the request processing chain before dispatching to endpoint handlers.
- **R-HONO-004** MUST: Restrict context storage to lightweight identifiers, validated authentication tokens, and essential tenant metadata to avoid memory overhead.

### Verify

```bash
# Discover and execute repository type-checking script
# Discover and execute repository static analysis and linting scripts
# Discover and run the project automated test suite
```

**Accept when:**
- All route handlers retrieve pre-authenticated tenant and telemetry metadata via context getter methods without re-querying datastores.
- Context variable keys and values conform to static type definitions verified by repository validation checks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>