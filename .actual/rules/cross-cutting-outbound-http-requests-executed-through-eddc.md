---
version: 2
name: cross-cutting-outbound-http-requests-executed-through-eddc
description: "Adoption of net/http for Service Networking and HTTP Client Communication: Outbound Http Requests Executed Through Client"
metadata:
  actual:
    adr_id: eddca1b2-ed49-4da4-ac95-c435734e4757
    status: accepted
    candidate_group_id: 5f922c0d-ec98-41d5-bfbf-3b766b41eda2
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - cmd/private/**
      - handlers/**
      - pkg/job/**
      - pkg/tinybird/**
      - proto/private_location/v1/**
---

# Adoption of net/http for Service Networking and HTTP Client Communication: Outbound Http Requests Executed Through Client

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** SHOULD: Outbound HTTP requests executed through http.Client.Do or RPC unary call wrappers SHOULD propagate context.Context instances to support cancellation and deadline enforcement.

### Verify

```bash
# Discover and execute project test suite to validate HTTP client timeout enforcement
# Discover and execute repository static analysis/linter configuration to verify no unbounded HTTP client instantiations exist
```

**Accept when:**
- All outbound HTTP client instances explicitly configure non-zero timeout values.
- Static analysis checks pass with zero warnings regarding unhandled response body closures or unconfigured client timeouts.
- Unit and integration test suites pass successfully across all service handlers and client packages.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>