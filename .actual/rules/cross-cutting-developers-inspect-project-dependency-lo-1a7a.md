---
version: 2
name: cross-cutting-developers-inspect-project-dependency-lo-1a7a
description: "Adoption of net/http for Service Networking and HTTP Client Communication: Developers Inspect Project Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 1a7a0361-a8b3-433b-90b7-4553053046b8
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

# Adoption of net/http for Service Networking and HTTP Client Communication: Developers Inspect Project Dependency Lock Artifact

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all service handlers, monitoring check jobs, telemetry client packages executing outbound HTTP requests, and internal client wrappers managing transport sessions.

### Rules

- **R-NET-001** MUST: Developers MUST inspect the project dependency lock artifact to verify the runtime environment version before implementing or modifying net/http client configurations.
- **R-NET-002** MUST: Instantiate http.Client instances with tailored Timeout durations derived from configuration sources or request parameters rather than sharing unconstrained clients.
- **R-NET-003** MUST: Ensure all network request invocations pass an active context to http.NewRequestWithContext or RPC client wrappers to honor process cancellation signals.
- **R-NET-004** MUST: Mandate immediate deferral of response body closure following error verification in all client execution paths.

### Verify

```bash
# Discover and run project test suite to validate HTTP client timeout enforcement
# Discover and execute repository static analysis/linter to check for unbounded HTTP clients
```

**Accept when:**
- All outbound HTTP client instances explicitly configure non-zero timeout values.
- Static analysis checks pass with zero warnings regarding unhandled response body closures or unconfigured client timeouts.
- Unit and integration test suites pass successfully across all service handlers and client packages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting and peer code reviews enforce these requirements, blocking merges and promotions upon violations.
</enforcement>