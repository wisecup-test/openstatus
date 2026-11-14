---
version: 2
name: cross-cutting-service-packages-not-introduce-external-421d
description: "net/http Core Library for HTTP Service Architecture and Client Transport: Service Packages Not Introduce External Web"
metadata:
  actual:
    adr_id: 421df4f2-30df-42f0-9d2e-6fb12a2c11e0
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

# net/http Core Library for HTTP Service Architecture and Client Transport: Service Packages Not Introduce External Web

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HTTP-001** MUST_NOT: Service packages MUST_NOT introduce external web frameworks or custom networking abstractions that bypass standard http.Handler and http.ResponseWriter interfaces.
- **R-HTTP-002** MUST: Instantiate outbound http.Client instances with bounded timeout settings and pass context.Context to all request builders.
- **R-HTTP-003** MUST: Implement response instrumentation by wrapping http.ResponseWriter to capture the HTTP status code before delegating to the underlying response writer.
- **R-HTTP-004** MUST: Structure server startup routines to bind listeners and invoke graceful shutdown procedures upon receiving termination signals.

### Verify

```bash
# Discover and execute the repository test suite to verify HTTP server handlers, client transports, and response writer implementations.
# Discover and run the repository static analysis and linting checks to ensure all HTTP clients configure explicit timeouts and pass context objects.
```

**Accept when:**
- All HTTP route handlers and client integration tests pass successfully without connection leaks or timeout errors.
- Static analysis verification confirms no unconfigured http.Client instances or context-less network calls exist.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>