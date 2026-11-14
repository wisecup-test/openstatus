---
version: 2
name: cross-cutting-components-requiring-http-communication-d018
description: "Adoption of net/http for Service Networking and HTTP Client Communication: Components Requiring Http Communication Adopt Net"
metadata:
  actual:
    adr_id: d0180f67-885f-4a4a-bfbb-de6b25000068
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

# Adoption of net/http for Service Networking and HTTP Client Communication: Components Requiring Http Communication Adopt Net

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** MUST: Components requiring HTTP communication MUST adopt net/http as their standard client transport and handler interface across service boundaries.
- **R-NET-002** MUST: Instantiate http.Client instances with tailored Timeout durations derived from configuration sources or request parameters rather than sharing unconstrained clients.
- **R-NET-003** MUST: Ensure all network request invocations pass an active context to http.NewRequestWithContext or RPC client wrappers to honor process cancellation signals.
- **R-NET-004** MUST: Mandate immediate deferral of response body closure following error verification in all client execution paths.

### Verify

```bash
# Discover and run the project test suite to validate HTTP client timeout enforcement
# Discover and execute the linter to verify that no unbounded HTTP client instantiations exist
```

**Accept when:**
- All outbound HTTP client instances explicitly configure non-zero timeout values.
- Static analysis checks pass with zero warnings regarding unhandled response body closures or unconfigured client timeouts.
- Unit and integration test suites pass successfully across all service handlers and client packages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting, static analysis checks, and peer code review verification are strictly enforced.
</enforcement>