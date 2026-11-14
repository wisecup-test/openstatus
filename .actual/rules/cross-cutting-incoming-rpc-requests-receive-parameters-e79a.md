---
version: 2
name: cross-cutting-incoming-rpc-requests-receive-parameters-e79a
description: "Adoption of Connect RPC for Service API Implementation: Incoming Rpc Requests Receive Parameters Wrapped"
metadata:
  actual:
    adr_id: e79adc5e-99de-4cd1-a51d-3e997d19cf0e
    status: accepted
    candidate_group_id: 12986db6-19ad-4261-b7b0-f7f63637c6f9
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of Connect RPC for Service API Implementation: Incoming Rpc Requests Receive Parameters Wrapped

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONNECT-001** MUST: Incoming RPC requests MUST receive parameters wrapped inside `connect.Request` generic types and extract headers and messages through its methods.

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