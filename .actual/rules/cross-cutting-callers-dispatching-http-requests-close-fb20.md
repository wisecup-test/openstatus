---
version: 2
name: cross-cutting-callers-dispatching-http-requests-close-fb20
description: "net/http Package for Outbound HTTP Client Integration: Callers Dispatching Http Requests Close Response"
metadata:
  actual:
    adr_id: fb20b283-2ece-464b-bb10-e144fedbfd87
    status: accepted
    candidate_group_id: b2869a74-327c-41e1-86c9-428c30e1d0a1
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/tinybird/**
      - internal/workflows/**
---

# net/http Package for Outbound HTTP Client Integration: Callers Dispatching Http Requests Close Response

Scope: applies to `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for outbound HTTP service integrations and API client adapters residing within internal application packages.

### Rules

- **R-HTTP-001** MUST: Callers dispatching HTTP requests MUST close the response body upon processing the response.

### Verify

```bash
# Discover the test execution entry point from repository configuration and run test suites covering outbound client packages.
go test ./...

# Discover the static analysis tool configured in the repository and verify compliance of all HTTP client implementations.
go vet ./...
```

**Accept when:**
- All outbound HTTP client implementations pass automated unit and integration tests verifying request construction, header injection, and context propagation.
- Static analysis and repository linters report zero violations regarding unclosed response bodies or unhandled context cancellation.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, code review checks, and repository test suites validating context deadline handling and response body closure.
</enforcement>