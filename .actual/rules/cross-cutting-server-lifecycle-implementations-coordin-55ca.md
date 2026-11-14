---
version: 2
name: cross-cutting-server-lifecycle-implementations-coordin-55ca
description: "net/http Core Library for HTTP Service Architecture and Client Transport: Server Lifecycle Implementations Coordinate Graceful Shutdown"
metadata:
  actual:
    adr_id: 55ca372e-1fe1-4f57-9d84-b4bbb09283d3
    status: accepted
    candidate_group_id: 1853f30f-7e00-4db1-8d56-6feed02d0ef4
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - cmd/server/**
      - internal/server/**
      - internal/tinybird/**
      - internal/workflows/**
      - proto/private_location/v1/**
---

# net/http Core Library for HTTP Service Architecture and Client Transport: Server Lifecycle Implementations Coordinate Graceful Shutdown

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all HTTP service architecture, entrypoints, routes, and client packages matching the configured scope.

### Rules

- **R-NET-001** SHOULD: Server lifecycle implementations SHOULD coordinate graceful shutdown with operating system signal listeners to drain active connections before process termination.
- **R-NET-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-NET-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock or resolution artifact for exact resolved version, look up official documentation/changelog/public API reference for that exact version (no training-data recall), confirm every API/class/function exists in that version, and re-run steps per dependency at point of use for version-sensitive behavior.
- **R-NET-004** MANDATORY: Instantiate outbound http.Client instances with bounded timeout settings and pass context.Context to all request builders.
- **R-NET-005** MANDATORY: Implement response instrumentation by wrapping http.ResponseWriter to capture the HTTP status code before delegating to the underlying response writer.
- **R-NET-006** MANDATORY: Structure server startup routines to bind listeners and invoke graceful shutdown procedures upon receiving termination signals.

### Verify

```bash
# Discover and execute the repository test suite to verify HTTP server handlers, client transports, and response writer implementations.
# Discover and run the repository static analysis and linting checks to ensure all HTTP clients configure explicit timeouts and pass context objects.
```

**Accept when:**
- All HTTP route handlers and client integration tests pass successfully without connection leaks or timeout errors.
- Static analysis verification confirms no unconfigured http.Client instances or context-less network calls exist.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration test validation suites, automated static analysis, and peer reviews verify compliance; pull requests introducing external web frameworks, missing client timeouts, or context-less network calls are blocked from merging.
</enforcement>