---
version: 2
name: cross-cutting-consumers-discover-repository-dependency-5df1
description: "Hono Framework Middleware and Context Management: Consumers Discover Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: 5df1182b-409b-44db-9d69-c633e95c57e1
    status: accepted
    candidate_group_id: 87aef6b0-858a-4df3-b448-91ee6ae3bf48
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
---

# Hono Framework Middleware and Context Management: Consumers Discover Repository Dependency Resolution Artifact

Scope: applies to `apps/server/src/libs/middlewares/**`.

These rules are ALWAYS ACTIVE for server HTTP middleware modules and route processing pipelines managing request lifecycle, authorization, and telemetry, as well as request-scoped state accessors resolving domain entities or client request headers.

### Rules

- **R-HONO-001** MUST: Consumers MUST discover the repository dependency resolution artifact to confirm the exact resolved version of the Hono library and verify API compatibility prior to implementing middleware handlers.

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