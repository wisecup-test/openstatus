---
version: 2
name: cross-cutting-payloads-sent-external-service-endpoints-4c15
description: "net/http Package for Outbound HTTP Client Integration: Payloads Sent External Service Endpoints Serialized"
metadata:
  actual:
    adr_id: 4c15162e-3e4c-4382-8f21-4692ba2683d9
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

# net/http Package for Outbound HTTP Client Integration: Payloads Sent External Service Endpoints Serialized

Scope: applies to `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for outbound HTTP service integrations and API client adapters residing within internal application packages.

### Rules

- **R-NET-001** MUST: Payloads sent to external service endpoints MUST be serialized into byte buffers using encoding/json before request dispatch.

### Verify

```bash
# Discover the test execution entry point from repository configuration and run test suites covering outbound client packages.
# Discover the static analysis tool configured in the repository and verify compliance of all HTTP client implementations.
```

**Accept when:**
- All outbound HTTP client implementations pass automated unit and integration tests verifying request construction, header injection, and context propagation.
- Static analysis and repository linters report zero violations regarding unclosed response bodies or unhandled context cancellation.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>