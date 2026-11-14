---
version: 2
name: cross-cutting-server-endpoints-transport-interceptors-f65f
description: "Adoption of @openstatus/error for Server Error Standardization: Server Endpoints Transport Interceptors Use Openstatus"
metadata:
  actual:
    adr_id: f65f59b8-248b-4997-92c4-4596713ca896
    status: accepted
    candidate_group_id: f4bfe830-6739-435c-8463-26208c562aeb
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/errors/**
      - src/routes/rpc/interceptors/**
---

# Adoption of @openstatus/error for Server Error Standardization: Server Endpoints Transport Interceptors Use Openstatus

Scope: applies to `apps/server/src/libs/errors/**`, `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** MUST: Server endpoints and transport interceptors MUST use @openstatus/error as the canonical source for error classification, error codes, and standardized error schemas across all client-facing protocols.

### Verify

```bash
# Discover and run the repository type-checking script across server packages
# Discover and execute the repository unit and integration test suite
# Execute the repository linting and static analysis suite
```

**Accept when:**
- Type checking passes without errors, confirming all referenced error codes and schema objects conform to @openstatus/error definitions.
- Unit and integration tests pass, confirming HTTP and RPC error responses emit standardized error codes and correlation identifiers.
- Static analysis validates that unexpected errors are intercepted and logged with correlated request identifiers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>