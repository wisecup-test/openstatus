---
version: 2
name: cross-cutting-background-forwarding-operations-spawned-ad20
description: "Adoption of Connect RPC for Service API Implementation: Background Forwarding Operations Spawned Request Handlers"
metadata:
  actual:
    adr_id: ad20ac2f-d4de-40d3-b7db-538b19c25ca0
    status: accepted
    candidate_group_id: 12986db6-19ad-4261-b7b0-f7f63637c6f9
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of Connect RPC for Service API Implementation: Background Forwarding Operations Spawned Request Handlers

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONN-001** SHOULD: Background forwarding operations spawned from request handlers that outlive the incoming transport lifecycle SHOULD derive detached contexts using `context.WithoutCancel` with explicit timeout boundaries.

### Verify

```bash
# Discover the project verification script from the repository configuration and execute the static analysis suite across all service handler packages.
# Discover and run the project test runner to execute unit and contract integration tests validating Connect RPC handler endpoints.
```

**Accept when:**
- All service handler functions compile successfully against resolved Connect RPC types and pass static analysis verification.
- All RPC endpoint test suites pass with zero failures, confirming valid response envelope construction and expected error status mapping for unauthenticated and internal failure conditions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines and pull request reviews enforce schema compilation, static analysis, and interface verification suites.
</enforcement>