---
version: 2
name: cross-cutting-procedure-declarations-derive-standardiz-4263
description: "Adoption of @trpc/server for Type-Safe API Procedure Architecture: Procedure Declarations Derive Standardized Base Instances"
metadata:
  actual:
    adr_id: 42637ead-18e8-45fa-bf1e-c9cfd611416e
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

# Adoption of @trpc/server for Type-Safe API Procedure Architecture: Procedure Declarations Derive Standardized Base Instances

Scope: applies to `packages/api/index.ts`, `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all API procedure declarations, router compositions, and endpoint service adapters within the API service package.

### Rules

- **R-PROC-001** MUST: All procedure declarations MUST derive from standardized base procedure instances configured with requisite context and timing middlewares.

### Verify

```bash
# Discover and execute dependency and manifest audit scripts to verify lock-file synchronization for server libraries
# Discover and execute static type analysis and linting scripts to verify compliance of all procedure and router definitions
# Discover and execute automated test suite covering API procedure execution, middleware behavior, and error handling paths
```

**Accept when:**
- All API router modules export procedures constructed using the adopted server procedure builders and middleware chains.
- All API errors originating within procedure handlers conform to standard TRPC error representations.
- Static analysis and build validation confirm zero untyped or disparate procedure declarations across the API package.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks validating static types and procedure contracts across the repository, and mandatory peer code review.
</enforcement>