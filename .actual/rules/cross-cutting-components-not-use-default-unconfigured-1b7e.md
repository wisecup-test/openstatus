---
version: 2
name: cross-cutting-components-not-use-default-unconfigured-1b7e
description: "Adoption of net/http for Service Networking and HTTP Client Communication: Components Not Use Default Unconfigured Http"
metadata:
  actual:
    adr_id: 1b7efd18-23e4-459e-9e0b-0ebe1f1f6923
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

# Adoption of net/http for Service Networking and HTTP Client Communication: Components Not Use Default Unconfigured Http

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all service handlers, monitoring check jobs, telemetry client packages, and internal client wrappers executing outbound HTTP requests or hosting RPC endpoints.

### Rules

- **R-NET-001** MUST_NOT: Components MUST_NOT use default unconfigured http.Client instances or package-level request functions with zero-value timeouts for outbound network probes.
- **R-NET-002** MANDATORY: Instantiate http.Client instances with tailored Timeout durations derived from configuration sources or request parameters rather than sharing unconstrained clients.
- **R-NET-003** MANDATORY: Ensure all network request invocations pass an active context to http.NewRequestWithContext or RPC client wrappers to honor process cancellation signals.

### Verify

```bash
# Discover and run the project test execution script from repository metadata
# Discover the repository static analysis configuration and execute the linter to verify no unbounded HTTP client instantiations exist
```

**Accept when:**
- All outbound HTTP client instances explicitly configure non-zero timeout values.
- Static analysis checks pass with zero warnings regarding unhandled response body closures or unconfigured client timeouts.
- Unit and integration test suites pass successfully across all service handlers and client packages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting and peer code review enforcement apply.
</enforcement>