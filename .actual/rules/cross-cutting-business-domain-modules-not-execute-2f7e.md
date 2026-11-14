---
version: 2
name: cross-cutting-business-domain-modules-not-execute-2f7e
description: "net/http Package for Outbound HTTP Client Integration: Business Domain Modules Not Execute Unencapsulated"
metadata:
  actual:
    adr_id: 2f7e1e0f-0173-4cc2-ae47-3669e2b23d81
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

# net/http Package for Outbound HTTP Client Integration: Business Domain Modules Not Execute Unencapsulated

Scope: applies to `apps/private-location/internal/tinybird/**`, `apps/private-location/internal/workflows/**`.

These rules are ALWAYS ACTIVE for all outbound service integrations and API client adapters residing within internal application packages.

### Rules

- **R-NET-001** MUST_NOT: Business domain modules MUST_NOT execute unencapsulated HTTP requests directly outside of dedicated client adapters.

### Verify

```bash
# Discover and run the test execution entry point for outbound client packages
# Discover and run the static analysis tool configured in the repository for HTTP client implementations
```

**Accept when:**
- All outbound HTTP client implementations pass automated unit and integration tests verifying request construction, header injection, and context propagation.
- Static analysis and repository linters report zero violations regarding unclosed response bodies or unhandled context cancellation.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis, code review checks, and repository test suites.
</enforcement>