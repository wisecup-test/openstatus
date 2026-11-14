---
version: 2
name: cross-cutting-network-request-operations-compose-http-188c
description: "Adoption of net/http for Service Networking and HTTP Client Communication: Network Request Operations Compose Http Client"
metadata:
  actual:
    adr_id: 188ce38f-fb0f-449b-a4a3-a889a43d3e93
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

# Adoption of net/http for Service Networking and HTTP Client Communication: Network Request Operations Compose Http Client

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NET-001** MAY: Network request operations MAY compose http.Client executions with exponential backoff retry mechanisms when communicating with resilient external ingestion endpoints.

### Verify

```bash
# Discover the project test execution script from repository metadata and run the test suite to validate HTTP client timeout enforcement.
# Discover the repository static analysis configuration and execute the linter to verify that no unbounded HTTP client instantiations exist.
```

**Accept when:**
- All outbound HTTP client instances explicitly configure non-zero timeout values.
- Static analysis checks pass with zero warnings regarding unhandled response body closures or unconfigured client timeouts.
- Unit and integration test suites pass successfully across all service handlers and client packages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting, static analysis checks, and peer code reviews enforce compliance.
</enforcement>