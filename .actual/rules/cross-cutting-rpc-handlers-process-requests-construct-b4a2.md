---
version: 2
name: cross-cutting-rpc-handlers-process-requests-construct-b4a2
description: "Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Rpc Handlers Process Requests Construct Responses"
metadata:
  actual:
    adr_id: b4a27dce-59af-4c7d-9631-5459b619394d
    status: accepted
    candidate_group_id: a63a9164-0f39-4ae0-992b-b56f3c02ef8f
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Rpc Handlers Process Requests Construct Responses

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for all server-side remote procedure call service routes and endpoint handlers, as well as request and response message conversions between internal models and RPC schemas.

### Rules

- **R-RPC-001** MUST: RPC handlers MUST process requests and construct responses conforming strictly to declared service protocol schemas without manual field transmutation.

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
Claude Code MUST NOT skip or defer verification. Continuous integration type checking, automated test execution, and peer code review strictly enforce these boundaries, blocking any pull requests that contain untyped HTTP endpoints or manual bypasses of the RPC framework.
</enforcement>