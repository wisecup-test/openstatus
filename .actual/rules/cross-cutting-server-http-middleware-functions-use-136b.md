---
version: 2
name: cross-cutting-server-http-middleware-functions-use-136b
description: "Hono Framework Middleware and Context Management: Server Http Middleware Functions Use Hono"
metadata:
  actual:
    adr_id: 136b2b7e-5582-4ddf-8ae7-eb8f448fbe3f
    status: accepted
    candidate_group_id: 87aef6b0-858a-4df3-b448-91ee6ae3bf48
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
---

# Hono Framework Middleware and Context Management: Server Http Middleware Functions Use Hono

Scope: applies to `apps/server/src/libs/middlewares/**`.

These rules are ALWAYS ACTIVE for Server HTTP middleware modules, route processing pipelines, and request-scoped state accessors managing request lifecycles, authorization, and telemetry.

### Rules

- **R-HONO-001** MUST: Server HTTP middleware functions MUST use the Hono framework context as the primary mechanism for accessing request headers and propagating request-scoped domain entities.

### Verify

```bash
# Discover the repository dependency manifest and execute the project type-checking script
# to confirm all Hono context accessors adhere to defined context type contracts.
# Discover and run the project test suite to verify middleware context propagation and exception handling.
```

**Accept when:**
- Static type analysis completes without errors across all middleware modules utilizing Hono context getters and request accessors.
- Automated test suites verify that scoped entities and headers propagate through the middleware pipeline and produce expected HTTP responses.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>