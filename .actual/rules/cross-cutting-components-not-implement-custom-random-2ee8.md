---
version: 2
name: cross-cutting-components-not-implement-custom-random-2ee8
description: "Adoption of nanoid for Unique Identifier Generation: Components Not Implement Custom Random String"
metadata:
  actual:
    adr_id: 2ee8e7a0-85cf-49b9-82eb-7396f451c991
    status: accepted
    candidate_group_id: a6d728a0-06cf-4329-9a4d-a16127e3c802
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Adoption of nanoid for Unique Identifier Generation: Components Not Implement Custom Random String

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ID-001** MUST_NOT: Components MUST_NOT implement custom random string generators or introduce alternative unique identifier libraries for server-side correlation keys and action tracking.

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