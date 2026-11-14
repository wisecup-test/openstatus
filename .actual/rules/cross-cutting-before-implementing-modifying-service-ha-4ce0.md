---
version: 2
name: cross-cutting-before-implementing-modifying-service-ha-4ce0
description: "Adoption of Connect RPC for Service API Implementation: Before Implementing Modifying Service Handlers That"
metadata:
  actual:
    adr_id: 4ce0c76c-9a1b-4d3e-b17d-14c4a179d975
    status: accepted
    candidate_group_id: 12986db6-19ad-4261-b7b0-f7f63637c6f9
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of Connect RPC for Service API Implementation: Before Implementing Modifying Service Handlers That

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONNECT-001** MUST: Before implementing or modifying service handlers that consume the Connect RPC library, engineers MUST inspect the repository dependency manifest and lock artifact to resolve the exact dependency version and verify API compatibility against authoritative documentation.

### Verify

```bash
# Discover and run the project verification script from repository configuration
# Run the static analysis suite across all service handler packages
# Discover and run the project test runner to execute unit and contract integration tests validating Connect RPC handler endpoints
```

**Accept when:**
- All service handler functions compile successfully against resolved Connect RPC types and pass static analysis verification.
- All RPC endpoint test suites pass with zero failures, confirming valid response envelope construction and expected error status mapping for unauthenticated and internal failure conditions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, schema compilation, static analysis, and interface verification suites enforce compliance.
</enforcement>