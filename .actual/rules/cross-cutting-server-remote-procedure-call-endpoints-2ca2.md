---
version: 2
name: cross-cutting-server-remote-procedure-call-endpoints-2ca2
description: "Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Server Remote Procedure Call Endpoints Defined"
metadata:
  actual:
    adr_id: 2ca29404-f052-4276-b837-252a78add27e
    status: accepted
    candidate_group_id: a63a9164-0f39-4ae0-992b-b56f3c02ef8f
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Server Remote Procedure Call Endpoints Defined

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RPC-001** MUST: Server remote procedure call endpoints MUST be defined, routed, and handled using the @connectrpc/connect framework and its associated schema bindings.

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
Claude Code MUST NOT skip or defer verification. All RPC endpoints must be verified for strict contract adherence, type safety, and correct framework usage.
</enforcement>