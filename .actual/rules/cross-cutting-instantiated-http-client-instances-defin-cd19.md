---
version: 2
name: cross-cutting-instantiated-http-client-instances-defin-cd19
description: "Adoption of net/http for Service Networking and HTTP Client Communication: Instantiated Http Client Instances Define Explicit"
metadata:
  actual:
    adr_id: cd19fa3f-8099-4eec-b430-7ac8063d569d
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

# Adoption of net/http for Service Networking and HTTP Client Communication: Instantiated Http Client Instances Define Explicit

Scope: applies to `apps/checker/cmd/private/**`, `apps/checker/handlers/**`, `apps/checker/pkg/job/**`, `apps/checker/pkg/tinybird/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all service handlers, monitoring check jobs, telemetry client packages executing outbound HTTP requests, and internal client wrappers managing transport sessions.

### Rules

- **R-HTTP-001** MUST: All instantiated http.Client instances MUST define an explicit non-zero Timeout attribute to prevent unbounded connection blocking.

### Verify

```bash
# Discover and run test suite via project metadata
# Discover and run static analysis linter to verify no unbounded HTTP client instantiations exist
```

**Accept when:**
- All outbound HTTP client instances explicitly configure non-zero timeout values.
- Static analysis checks pass with zero warnings regarding unhandled response body closures or unconfigured client timeouts.
- Unit and integration test suites pass successfully across all service handlers and client packages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated linting, static analysis checks, and peer code review verification are mandatory for all client instantiations and network handlers.
</enforcement>