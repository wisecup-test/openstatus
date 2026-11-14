---
version: 2
name: cross-cutting-remote-service-handler-endpoints-probe-bfb7
description: "Adoption of Connect RPC for Service API Implementation: Remote Service Handler Endpoints Probe Querying"
metadata:
  actual:
    adr_id: bfb74cdf-43fe-4a8b-96dd-aa4ae78b8b63
    status: accepted
    candidate_group_id: 12986db6-19ad-4261-b7b0-f7f63637c6f9
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of Connect RPC for Service API Implementation: Remote Service Handler Endpoints Probe Querying

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all remote service handler endpoints for probe querying and telemetry ingestion.

### Rules

- **R-CON-001** MUST: All remote service handler endpoints for probe querying and telemetry ingestion MUST be implemented using the Connect RPC library with generic request and response envelope types.

### Verify

```bash
# Discover and execute the project verification script from repository configuration for static analysis
# Discover and run the project test runner to execute unit and contract integration tests
```

**Accept when:**
- All service handler functions compile successfully against resolved Connect RPC types and pass static analysis verification.
- All RPC endpoint test suites pass with zero failures, confirming valid response envelope construction and expected error status mapping for unauthenticated and internal failure conditions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>