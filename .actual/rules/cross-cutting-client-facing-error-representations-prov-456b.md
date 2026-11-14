---
version: 2
name: cross-cutting-client-facing-error-representations-prov-456b
description: "Adoption of @openstatus/error for Server Error Standardization: Client Facing Error Representations Provide Documentation"
metadata:
  actual:
    adr_id: 456b09b4-07a3-416b-8c84-d036b66a11e8
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

# Adoption of @openstatus/error for Server Error Standardization: Client Facing Error Representations Provide Documentation

Scope: applies to `apps/server/src/libs/errors/**`, `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** SHOULD: Client-facing error representations SHOULD provide documentation references corresponding to the specific error code defined in @openstatus/error to assist external debugging.

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