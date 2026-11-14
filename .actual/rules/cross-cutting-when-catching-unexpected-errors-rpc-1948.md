---
version: 2
name: cross-cutting-when-catching-unexpected-errors-rpc-1948
description: "Adoption of @openstatus/error for Server Error Standardization: When Catching Unexpected Errors Rpc Server"
metadata:
  actual:
    adr_id: 1948030e-36f5-44d4-9d8d-be893afc48e4
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

# Adoption of @openstatus/error for Server Error Standardization: When Catching Unexpected Errors Rpc Server

Scope: applies to `apps/server/src/libs/errors/**`, `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all server route handlers, RPC procedure implementations, transport interceptors emitting error responses, and error utility modules.

### Rules

- **R-OPENSTATUS-001** MUST: When catching unexpected errors or RPC server errors, interceptors and handlers MUST record structured log entries containing error details alongside the request context correlation identifier.
- **R-OPENSTATUS-002** MUST: Define new error classifications directly within @openstatus/error to maintain centralized cataloging before exposing them across service endpoints.
- **R-OPENSTATUS-003** MUST: Ensure RPC interceptors and HTTP error middleware consistently attach request correlation identifiers to both structured log records and outgoing error payloads.

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