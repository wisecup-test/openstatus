---
version: 2
name: cross-cutting-public-facing-procedures-declare-explici-541d
description: "@trpc/server API Service Boundary Definition: Public Facing Procedures Declare Explicit Output"
metadata:
  actual:
    adr_id: 541dca2d-7509-43a8-ae1a-fa0774d8a6aa
    status: accepted
    candidate_group_id: 1c9c1530-10ff-44aa-8e8f-5c16d37ddbc1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server API Service Boundary Definition: Public Facing Procedures Declare Explicit Output

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** SHOULD: Public-facing procedures SHOULD declare explicit output validation schemas using the output builder method to guarantee response contract stability and prevent data leakage.

### Verify

```bash
# Discover and execute project test suite for API procedures
npm test
# Discover and run static type checking across the API package
npm run typecheck
```

**Accept when:**
- All API procedures verify compile-time type safety across input, output, and context boundaries.
- Static analysis confirms schema validation is attached to all externally reachable procedure inputs.
- Middleware execution pipelines enforce timing and authentication constraints across all procedure invocations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>