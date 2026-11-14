---
version: 2
name: cross-cutting-error-schemas-serialization-logic-incorp-af49
description: "Adoption of @openstatus/error for Server Error Standardization: Error Schemas Serialization Logic Incorporate Code"
metadata:
  actual:
    adr_id: af49d291-d12c-4fda-8f5f-6edd9e7ca35a
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

# Adoption of @openstatus/error for Server Error Standardization: Error Schemas Serialization Logic Incorporate Code

Scope: applies to `apps/server/src/libs/errors/**`, `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ERR-001** MUST: Error schemas and error serialization logic MUST incorporate error code enumerations from @openstatus/error and associate each error with a correlation identifier extracted from the request context.

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