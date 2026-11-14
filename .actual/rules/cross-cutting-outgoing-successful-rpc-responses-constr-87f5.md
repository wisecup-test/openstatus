---
version: 2
name: cross-cutting-outgoing-successful-rpc-responses-constr-87f5
description: "Adoption of Connect RPC for Service API Implementation: Outgoing Successful Rpc Responses Constructed Connect"
metadata:
  actual:
    adr_id: 87f53893-03de-4a79-954b-f4c39a9ff5e9
    status: accepted
    candidate_group_id: 12986db6-19ad-4261-b7b0-f7f63637c6f9
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of Connect RPC for Service API Implementation: Outgoing Successful Rpc Responses Constructed Connect

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONN-001** MUST: Outgoing successful RPC responses MUST be constructed using `connect.NewResponse` generic constructors wrapping strongly-typed response messages.

### Verify

```bash
# Discover the project verification script from the repository configuration and execute the static analysis suite across all service handler packages.
# Discover and run the project test runner to execute unit and contract integration tests validating Connect RPC handler endpoints.
```

**Accept when:**
- All service handler functions compile successfully against resolved Connect RPC types and pass static analysis verification.
- All RPC endpoint test suites pass with zero failures, confirming valid response envelope construction and expected error status mapping for unauthenticated and internal failure conditions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>