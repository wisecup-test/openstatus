---
version: 2
name: cross-cutting-procedures-encountering-non-recoverable-168d
description: "Adoption of @trpc/server for Type-Safe API Procedure Architecture: Procedures Encountering Non Recoverable Domain Conditions"
metadata:
  actual:
    adr_id: 168d7252-c5d5-4214-b1ee-29a897338dd4
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

# Adoption of @trpc/server for Type-Safe API Procedure Architecture: Procedures Encountering Non Recoverable Domain Conditions

Scope: applies to `packages/api/index.ts`, `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Procedures encountering non-recoverable domain conditions or rejected operations MUST throw or return TRPCError instances with appropriate semantic error codes.

### Verify

```bash
# Discover and execute dependency and manifest audit scripts to verify lock-file synchronization for server libraries
# Discover and execute static type analysis and linting scripts to verify procedure and router definitions
# Discover and execute automated test suite covering API procedure execution, middleware behavior, and error handling
```

**Accept when:**
- All API router modules export procedures constructed using the adopted server procedure builders and middleware chains.
- All API errors originating within procedure handlers conform to standard TRPC error representations.
- Static analysis and build validation confirm zero untyped or disparate procedure declarations across the API package.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing raw unmanaged endpoints or non-standard procedure definitions will fail automated lint and type validation checks and will be blocked from merging.
</enforcement>