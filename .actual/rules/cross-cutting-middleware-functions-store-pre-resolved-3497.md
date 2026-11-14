---
version: 2
name: cross-cutting-middleware-functions-store-pre-resolved-3497
description: "Hono Request Context Storage for Ephemeral State Propagation: Middleware Functions Store Pre Resolved Authentication"
metadata:
  actual:
    adr_id: 3497fb07-ddcc-49fc-8e34-b916af7d6437
    status: accepted
    candidate_group_id: 53cdfaf2-4e06-4c56-a2ed-67f5e36d7af3
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Request Context Storage for Ephemeral State Propagation: Middleware Functions Store Pre Resolved Authentication

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: Middleware functions MUST store pre-resolved authentication credentials, workspace tenant identifiers, and telemetry context using context setter methods prior to passing execution downstream.

### Verify

```bash
# Discover and execute repository type-checking script from the project manifest
# Discover and execute repository static analysis and linting scripts
# Discover and run project automated test suite
```

**Accept when:**
- All route handlers retrieve pre-authenticated tenant and telemetry metadata via context getter methods without re-querying datastores.
- Context variable keys and values conform to static type definitions verified by repository validation checks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>