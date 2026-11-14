---
version: 2
name: cross-cutting-when-introducing-updating-versioned-depe-288d
description: "net/http Package for Outbound HTTP Client Integration: When Introducing Updating Versioned Dependencies Network"
metadata:
  actual:
    adr_id: 288dd36f-023a-4030-83fe-0b828fa97dce
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

# net/http Package for Outbound HTTP Client Integration: When Introducing Updating Versioned Dependencies Network

Scope: applies to `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for outbound service integrations, API client adapters residing within internal application packages, and when introducing or updating versioned dependencies for network integrations.

### Rules

- **R-NET-001** MUST: When introducing or updating versioned dependencies for network integrations, the consumer MUST inspect the repository lock artifact and verify the exact resolved dependency version prior to implementation.
- **R-NET-002** MUST: Encapsulate net/http Client references within internal client structures initialized through dedicated constructor functions.
- **R-NET-003** MUST: Always inject authorization credentials into request headers during request construction rather than exposing them in query parameters.
- **R-NET-004** MUST: Ensure response bodies are consistently closed after inspecting execution errors.

### Verify

```bash
# Discover and run test suites covering outbound client packages
# Discover and run the static analysis tool configured in the repository to verify HTTP client implementations
```

**Accept when:**
- All outbound HTTP client implementations pass automated unit and integration tests verifying request construction, header injection, and context propagation.
- Static analysis and repository linters report zero violations regarding unclosed response bodies or unhandled context cancellation.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated static analysis, code review checks, and repository test suites is mandatory.
</enforcement>