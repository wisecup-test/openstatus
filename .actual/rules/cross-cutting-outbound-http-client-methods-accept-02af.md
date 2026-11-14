---
version: 2
name: cross-cutting-outbound-http-client-methods-accept-02af
description: "net/http Package for Outbound HTTP Client Integration: Outbound Http Client Methods Accept Context"
metadata:
  actual:
    adr_id: 02afb3b6-df5b-46eb-8779-bc75f5dc1bf2
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

# net/http Package for Outbound HTTP Client Integration: Outbound Http Client Methods Accept Context

Scope: applies to `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for outbound HTTP service integrations and API client adapters residing within internal application packages.

### Rules

- **R-HTTP-001** MUST: Outbound HTTP client methods MUST accept context.Context to propagate deadlines and cancellation signals to the underlying request.

### Verify

```bash
# Discover the test execution entry point from repository configuration and run test suites covering outbound client packages.
# Discover the static analysis tool configured in the repository and verify compliance of all HTTP client implementations.
```

**Accept when:**
- All outbound HTTP client implementations pass automated unit and integration tests verifying request construction, header injection, and context propagation.
- Static analysis and repository linters report zero violations regarding unclosed response bodies or unhandled context cancellation.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>