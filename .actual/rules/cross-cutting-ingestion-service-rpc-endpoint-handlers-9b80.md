---
version: 2
name: cross-cutting-ingestion-service-rpc-endpoint-handlers-9b80
description: "Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Ingestion Service Rpc Endpoint Handlers Use"
metadata:
  actual:
    adr_id: 9b802378-7fc4-4a69-8b4a-c66ea2ec538a
    status: accepted
    candidate_group_id: a5f353ac-efb9-4b32-b33a-11444dec2ad5
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Ingestion Service Rpc Endpoint Handlers Use

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ING-001** MUST: Ingestion service RPC endpoint handlers MUST use connectrpc.com/connect to define endpoint signatures with generic request and response wrappers.

### Verify

```bash
# Discover the project test execution harness and run the test suite for all ingestion endpoint handlers.
# Discover the project code analysis linter to verify that all RPC handlers conform to ConnectRPC handler signatures.
```

**Accept when:**
- All ingestion endpoint tests pass successfully and validate protocol compliance.
- Static analysis confirms handler signatures adhere to connectrpc.com/connect request and response wrappers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>