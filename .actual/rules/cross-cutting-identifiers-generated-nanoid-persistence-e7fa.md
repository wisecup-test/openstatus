---
version: 2
name: cross-cutting-identifiers-generated-nanoid-persistence-e7fa
description: "Adoption of nanoid for Unique Identifier Generation: Identifiers Generated Nanoid Persistence Keys Action"
metadata:
  actual:
    adr_id: e7fa169b-bdf3-468e-93b9-6656a3a4eb2f
    status: accepted
    candidate_group_id: a6d728a0-06cf-4329-9a4d-a16127e3c802
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of nanoid for Unique Identifier Generation: Identifiers Generated Nanoid Persistence Keys Action

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NANO-001** MUST: Identifiers generated with nanoid for persistence keys, action tokens, or context headers MUST be stored and transmitted as string types.

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