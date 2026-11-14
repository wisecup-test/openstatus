---
version: 2
name: cross-cutting-downstream-datastore-failures-inside-pub-0b98
description: "@trpc/server API Service Boundary Definition: Downstream Datastore Failures Inside Public Procedures"
metadata:
  actual:
    adr_id: 0b9890d1-ee96-431c-9c17-43f557972c1c
    status: accepted
    candidate_group_id: 1c9c1530-10ff-44aa-8e8f-5c16d37ddbc1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server API Service Boundary Definition: Downstream Datastore Failures Inside Public Procedures

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** SHOULD: Downstream datastore failures inside public procedures SHOULD gracefully degrade to default or fallback structures rather than emitting unhandled internal exceptions.

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