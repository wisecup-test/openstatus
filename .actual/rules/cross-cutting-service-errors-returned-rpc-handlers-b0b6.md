---
version: 2
name: cross-cutting-service-errors-returned-rpc-handlers-b0b6
description: "Adoption of Connect RPC for Service API Implementation: Service Errors Returned Rpc Handlers Constructed"
metadata:
  actual:
    adr_id: b0b6a092-10c4-484d-a2ca-94c4ba6bf080
    status: accepted
    candidate_group_id: 12986db6-19ad-4261-b7b0-f7f63637c6f9
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of Connect RPC for Service API Implementation: Service Errors Returned Rpc Handlers Constructed

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for server-side RPC handler functions processing monitoring queries and telemetry ingestion across service boundaries, and internal communication protocols between private location runners and core ingestion infrastructure.

### Rules

- **R-CONN-001** MUST: Service errors returned from RPC handlers MUST be constructed using `connect.NewError` paired with explicit Connect status codes matching the failure condition.

### Verify

```bash
# Discover and run the project static analysis suite and test runner across service handler packages
```

**Accept when:**
- All service handler functions compile successfully against resolved Connect RPC types and pass static analysis verification.
- All RPC endpoint test suites pass with zero failures, confirming valid response envelope construction and expected error status mapping for unauthenticated and internal failure conditions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipelines and architectural review during pull request inspection.
</enforcement>