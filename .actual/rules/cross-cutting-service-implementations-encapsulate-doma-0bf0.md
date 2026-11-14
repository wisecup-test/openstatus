---
version: 2
name: cross-cutting-service-implementations-encapsulate-doma-0bf0
description: "Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Service Implementations Encapsulate Domain Logic Separate"
metadata:
  actual:
    adr_id: 0bf09c03-1177-4da3-9bf7-4a8e36e2e3ee
    status: accepted
    candidate_group_id: a63a9164-0f39-4ae0-992b-b56f3c02ef8f
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Service Implementations Encapsulate Domain Logic Separate

Scope: applies to `apps/server/src/routes/rpc/**`.

These rules are ALWAYS ACTIVE for all server-side remote procedure call service routes and endpoint handlers, and request and response message conversions between internal models and RPC schemas.

### Rules

- **R-CONNECT-001** SHOULD: Service implementations SHOULD encapsulate domain logic in separate service modules rather than inlining business processes directly into RPC routing callbacks.

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
Claude Code MUST NOT skip or defer verification. Continuous integration type checking and automated test execution must verify all pull requests.
</enforcement>