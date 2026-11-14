---
version: 2
name: cross-cutting-before-introducing-updating-any-dependen-081f
description: "net/http Core Library for HTTP Service Architecture and Client Transport: Before Introducing Updating Any Dependency Associated"
metadata:
  actual:
    adr_id: 081fb3fd-aa06-494c-853b-ec920ed5b206
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

# net/http Core Library for HTTP Service Architecture and Client Transport: Before Introducing Updating Any Dependency Associated

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for HTTP server initialization, route definitions, response handling, and outbound HTTP API client integrations.

### Rules

- **R-NET-001** MUST: Before introducing or updating any dependency associated with the networking transport layer, the consumer MUST inspect the repository dependency manifest and lock artifact to resolve the exact locked dependency version against current official documentation.

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