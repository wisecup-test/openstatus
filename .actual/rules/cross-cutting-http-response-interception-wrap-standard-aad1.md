---
version: 2
name: cross-cutting-http-response-interception-wrap-standard-aad1
description: "net/http Core Library for HTTP Service Architecture and Client Transport: Http Response Interception Wrap Standard Responsewriter"
metadata:
  actual:
    adr_id: aad1b57f-0e41-4ef1-8fab-208012a4d49b
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

# net/http Core Library for HTTP Service Architecture and Client Transport: Http Response Interception Wrap Standard Responsewriter

Scope: applies to `apps/private-location/cmd/server/**`, `apps/private-location/internal/server/**`, `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HTTP-001** MUST: HTTP response interception MUST wrap the standard http.ResponseWriter interface and ensure proper default status code assignment.

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