---
version: 2
name: cross-cutting-asynchronous-workflows-cache-management-308f
description: "Adoption of nanoid for Unique Identifier Generation: Asynchronous Workflows Cache Management Routines Use"
metadata:
  actual:
    adr_id: 308f8bf5-e131-4f2c-a2b3-0dee5bea38d9
    status: accepted
    candidate_group_id: a6d728a0-06cf-4329-9a4d-a16127e3c802
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of nanoid for Unique Identifier Generation: Asynchronous Workflows Cache Management Routines Use

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NANOID-001** SHOULD: Asynchronous workflows and cache management routines SHOULD use nanoid-generated keys with prefixed namespace strings to prevent key collisions across domain entities.

### Verify

```bash
# Discover the repository build configuration and execute the project test suite to validate identifier generation and cache key resolution.
# Execute the repository static analysis and type verification routines to ensure all identifier consumers conform to the library interface.
```

**Accept when:**
- All unit and integration tests exercising asynchronous action stores and RPC interceptors pass without identifier generation errors.
- Static type verification confirms nanoid is imported and called in accordance with its resolved library type definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>