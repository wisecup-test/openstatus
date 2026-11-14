---
version: 2
name: cross-cutting-before-implementing-updating-any-procedu-74e8
description: "@trpc/server API Service Boundary Definition: Before Implementing Updating Any Procedure Consumer"
metadata:
  actual:
    adr_id: 74e826d2-71ce-4d7c-a617-ea90b0b56685
    status: accepted
    candidate_group_id: 1c9c1530-10ff-44aa-8e8f-5c16d37ddbc1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server API Service Boundary Definition: Before Implementing Updating Any Procedure Consumer

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Before implementing or updating any API procedure, the consumer MUST inspect the repository lock artifact and verify API compatibility against the exact resolved version of @trpc/server.

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