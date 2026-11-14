---
version: 2
name: cross-cutting-outbound-http-service-integrations-encap-8f40
description: "net/http Package for Outbound HTTP Client Integration: Outbound Http Service Integrations Encapsulate Communication"
metadata:
  actual:
    adr_id: 8f40e605-97c8-416c-a7c3-0b7c61b036e8
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

# net/http Package for Outbound HTTP Client Integration: Outbound Http Service Integrations Encapsulate Communication

Scope: applies to `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for outbound HTTP service integrations and API client adapters residing within internal application packages.

### Rules

- **R-HTTP-001** MUST: Outbound HTTP service integrations MUST encapsulate communication within dedicated client structs utilizing the standard net/http package and dispatch requests via Client.Do.

### Verify

```bash
# Discover the test execution entry point from repository configuration and run test suites covering outbound client packages.
go test ./...

# Discover the static analysis tool configured in the repository and verify compliance of all HTTP client implementations.
which golangci-lint && golangci-lint run
```

**Accept when:**
- All outbound HTTP client implementations pass automated unit and integration tests verifying request construction, header injection, and context propagation.
- Static analysis and repository linters report zero violations regarding unclosed response bodies or unhandled context cancellation.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and code review checks verify that external calls route through encapsulated client structs, and repository test suites validate context deadline handling and response body closure.
</enforcement>