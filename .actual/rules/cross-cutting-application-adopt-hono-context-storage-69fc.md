---
version: 2
name: cross-cutting-application-adopt-hono-context-storage-69fc
description: "Hono Request Context Storage for Ephemeral State Propagation: Application Adopt Hono Context Storage Methods"
metadata:
  actual:
    adr_id: 69fc779a-9fdc-4547-9cb2-8b3b2191d717
    status: accepted
    candidate_group_id: 53cdfaf2-4e06-4c56-a2ed-67f5e36d7af3
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Request Context Storage for Ephemeral State Propagation: Application Adopt Hono Context Storage Methods

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for HTTP server request processing pipelines, middleware functions, and route handlers managing cross-cutting request-scoped data propagation.

### Rules

- **R-HONO-001** MUST: The application MUST adopt Hono Context storage methods to manage ephemeral request-scoped state across middleware and route handler lifecycles.
- **R-HONO-002** MUST: Execute lock-version grounding before writing code using a versioned library: find dependency manifest, identify build tool, inspect lock/resolution artifact for exact version, look up official documentation for that exact version, confirm APIs exist, and re-run per dependency at point of use.
- **R-HONO-003** MUST: Define a centralized context variable interface when initializing the HTTP application to provide type safety across all setter and getter invocations.
- **R-HONO-004** MUST: Sequence authentication and tracing middlewares at the head of the request processing chain before dispatching to endpoint handlers.
- **R-HONO-005** MUST: Restrict context storage to lightweight identifiers, validated authentication tokens, and essential tenant metadata to prevent excessive memory overhead.

### Verify

```bash
# Discover and execute the repository type-checking script from the project manifest
# Discover and execute the repository static analysis and linting scripts
# Discover and run the project automated test suite
```

**Accept when:**
- All route handlers retrieve pre-authenticated tenant and telemetry metadata via context getter methods without re-querying datastores.
- Context variable keys and values conform to static type definitions verified by repository validation checks.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests must pass static type analysis, linting, and automated test suites verifying context state usage.
</enforcement>