---
version: 2
name: cross-cutting-before-invoking-any-nanoid-generation-dd77
description: "Adoption of nanoid for Unique Identifier Generation: Before Invoking Any Nanoid Generation Functions"
metadata:
  actual:
    adr_id: dd77de1a-2061-4651-9e13-b2ed1da59871
    status: accepted
    candidate_group_id: a6d728a0-06cf-4329-9a4d-a16127e3c802
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of nanoid for Unique Identifier Generation: Before Invoking Any Nanoid Generation Functions

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NANOID-001** MUST: Before invoking any nanoid generation functions, engineers MUST discover the repository lock artifact and verify the exact resolved version to ensure compatibility with runtime constraints.

### Verify

```bash
# Discover the repository build configuration and execute the project test suite to validate identifier generation and cache key resolution.
# Execute the repository static analysis and type verification routines to ensure all identifier consumers conform to the library interface.
```

**Accept when:**
- All unit and integration tests exercising asynchronous action stores and RPC interceptors pass without identifier generation errors.
- Static type verification confirms nanoid is imported and called in accordance with its resolved library type definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing alternative identifier generators or unverified custom random functions are blocked until refactored to use nanoid.
</enforcement>