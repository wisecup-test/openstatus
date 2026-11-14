---
version: 2
name: cross-cutting-handlers-not-bypass-schema-deserializati-bf04
description: "Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Handlers Not Bypass Schema Deserialization Accepting"
metadata:
  actual:
    adr_id: bf04d081-e7f9-460a-b2b0-309494856a97
    status: accepted
    candidate_group_id: a63a9164-0f39-4ae0-992b-b56f3c02ef8f
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Handlers Not Bypass Schema Deserialization Accepting

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONNECT-001** MUST_NOT: Handlers MUST_NOT bypass schema deserialization by accepting raw untyped payload bodies for RPC endpoints managed by the framework.

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