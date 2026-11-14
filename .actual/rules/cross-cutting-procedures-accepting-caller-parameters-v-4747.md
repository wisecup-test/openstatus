---
version: 2
name: cross-cutting-procedures-accepting-caller-parameters-v-4747
description: "Adoption of @trpc/server for Type-Safe API Procedure Architecture: Procedures Accepting Caller Parameters Validate Input"
metadata:
  actual:
    adr_id: 4747b6d6-de02-46ee-98e3-eb52134717d0
    status: accepted
    candidate_group_id: 339fa5ae-e0d7-4a41-b1e8-305ba484af35
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - index.ts
      - src/**
---

# Adoption of @trpc/server for Type-Safe API Procedure Architecture: Procedures Accepting Caller Parameters Validate Input

Scope: applies to `packages/api/index.ts`, `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all API procedure declarations, router compositions, and endpoint service adapters within the API service package.

### Rules

- **R-TRPC-001** SHOULD: Procedures accepting caller parameters SHOULD validate input payloads using schema validation at the procedure input boundary.

### Verify

```bash
# Discover and execute the project dependency and manifest audit scripts to verify lock-file synchronization for all server libraries.
# Discover and execute the project static type analysis and linting scripts to verify compliance of all procedure and router definitions.
# Discover and execute the project automated test suite covering API procedure execution, middleware behavior, and error handling paths.
```

**Accept when:**
- All API router modules export procedures constructed using the adopted server procedure builders and middleware chains.
- All API errors originating within procedure handlers conform to standard TRPC error representations.
- Static analysis and build validation confirm zero untyped or disparate procedure declarations across the API package.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>