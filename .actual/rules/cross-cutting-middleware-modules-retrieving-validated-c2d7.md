---
version: 2
name: cross-cutting-middleware-modules-retrieving-validated-c2d7
description: "Hono Framework Middleware and Context Management: Middleware Modules Retrieving Validated Domain Entities"
metadata:
  actual:
    adr_id: c2d71dca-e738-461d-b514-64bd6928273f
    status: accepted
    candidate_group_id: 87aef6b0-858a-4df3-b448-91ee6ae3bf48
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
---

# Hono Framework Middleware and Context Management: Middleware Modules Retrieving Validated Domain Entities

Scope: applies to `apps/server/src/libs/middlewares/**`.

These rules are ALWAYS ACTIVE for Server HTTP middleware modules and route processing pipelines managing request lifecycle, authorization, and telemetry, as well as request-scoped state accessors resolving domain entities or client request headers.

### Rules

- **R-HONO-001** MUST: Middleware modules retrieving validated domain entities MUST obtain them via Hono context accessor methods rather than mutating global state or directly re-evaluating raw request parameters.

### Verify

```bash
# Discover the repository dependency manifest and execute the project type-checking script to confirm all Hono context accessors adhere to defined context type contracts.
# Discover and run the project test suite to verify that middleware context propagation and HTTP exception handling pass all automated test scenarios.
```

**Accept when:**
- Static type analysis completes without errors across all middleware modules utilizing Hono context getters and request accessors.
- Automated test suites verify that scoped entities and headers propagate through the middleware pipeline and produce expected HTTP responses.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>