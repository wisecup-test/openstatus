---
version: 2
name: cross-cutting-serialization-payload-normalization-erro-7d21
description: "Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Serialization Payload Normalization Errors Encountered During"
metadata:
  actual:
    adr_id: 7d21ed9c-0994-406e-94e4-e74443cff981
    status: accepted
    candidate_group_id: a63a9164-0f39-4ae0-992b-b56f3c02ef8f
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Serialization Payload Normalization Errors Encountered During

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for all server-side remote procedure call service routes and endpoint handlers, request and response message conversions between internal models and RPC schemas.

### Rules

- **R-CONN-001** SHOULD: Serialization and payload normalization errors encountered during protocol translation SHOULD be caught and logged with structured context prior to returning protocol-compliant error codes.

### Verify

```bash
# Discover and run the project type-checking script using the designated repository runner to verify type safety across all RPC routes and handlers.
# Discover and execute the test runner target for handler unit and integration suites to confirm schema compliance and error handling.
# Execute the repository linting suite to ensure import structures and handler conventions adhere to project standards.
```

**Accept when:**
- The repository type checker completes with zero errors across all RPC routing and handler modules.
- All test suites for RPC route registration and message conversion pass successfully.
- No untyped or ad-hoc request body parsers bypass the RPC framework boundary.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>