---
version: 2
name: cross-cutting-http-server-implementations-inbound-rout-ee15
description: "net/http Core Library for HTTP Service Architecture and Client Transport: Http Server Implementations Inbound Route Handlers"
metadata:
  actual:
    adr_id: ee15e6e6-f8f0-4f24-b33c-352d77a64133
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

# net/http Core Library for HTTP Service Architecture and Client Transport: Http Server Implementations Inbound Route Handlers

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all HTTP server implementations, inbound route handlers, and outbound HTTP clients within the service.

### Rules

- **R-NET-001** MUST: All HTTP server implementations, inbound route handlers, and outbound HTTP clients within the service MUST use the standard net/http library for transport and request processing.
- **R-NET-002** MUST: Instantiate outbound http.Client instances with bounded timeout settings and pass context.Context to all request builders.
- **R-NET-003** MUST: Implement response instrumentation by wrapping http.ResponseWriter to capture the HTTP status code before delegating to the underlying response writer.
- **R-NET-004** MUST: Structure server startup routines to bind listeners and invoke graceful shutdown procedures upon receiving termination signals.

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