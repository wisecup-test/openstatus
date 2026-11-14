---
version: 2
name: cross-cutting-application-establish-statically-typed-c-14f8
description: "Hono Request Context Storage for Ephemeral State Propagation: Application Establish Statically Typed Context Variable"
metadata:
  actual:
    adr_id: 14f89faa-f2ab-44b9-8d0a-97a9b74cd6c3
    status: accepted
    candidate_group_id: 53cdfaf2-4e06-4c56-a2ed-67f5e36d7af3
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Request Context Storage for Ephemeral State Propagation: Application Establish Statically Typed Context Variable

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for HTTP server request processing pipelines, middleware functions, and route handlers involving cross-cutting request-scoped data propagation.

### Rules

- **R-HONO-001** SHOULD: The application SHOULD establish statically typed context variable schemas when instantiating the HTTP framework to enforce type safety on context keys and retrieved values.

### Verify

```bash
# Discover and execute repository type-checking script from the project manifest
# Discover and execute repository static analysis and linting scripts
# Discover and run the project automated test suite
```

**Accept when:**
- All route handlers retrieve pre-authenticated tenant and telemetry metadata via context getter methods without re-querying datastores.
- Context variable keys and values conform to static type definitions verified by repository validation checks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by static type analysis, automated test suites, and peer code review is mandatory.
</enforcement>