---
version: 2
name: cross-cutting-procedure-resolvers-not-extract-raw-1620
description: "@trpc/server API Service Boundary Definition: Procedure Resolvers Not Extract Raw Transport"
metadata:
  actual:
    adr_id: 1620e971-e686-45b3-973a-dbba7fdfd252
    status: accepted
    candidate_group_id: 1c9c1530-10ff-44aa-8e8f-5c16d37ddbc1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server API Service Boundary Definition: Procedure Resolvers Not Extract Raw Transport

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all API router modules, procedure definitions, and middleware chains defined within the API boundary package.

### Rules

- **R-TRPC-001** MUST_NOT: Procedure resolvers MUST NOT extract raw transport headers or cookies directly within procedure logic, relying instead on strongly typed context populated during initialization.

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
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration type checking and lint rules verifying procedure definitions and schema bindings.
</enforcement>