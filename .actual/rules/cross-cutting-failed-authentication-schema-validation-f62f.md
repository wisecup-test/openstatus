---
version: 2
name: cross-cutting-failed-authentication-schema-validation-f62f
description: "Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Failed Authentication Schema Validation Failures Downstream"
metadata:
  actual:
    adr_id: f62f96cd-0942-466e-b73e-03c244d38427
    status: accepted
    candidate_group_id: a5f353ac-efb9-4b32-b33a-11444dec2ad5
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Failed Authentication Schema Validation Failures Downstream

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONN-001** MUST: Failed authentication, schema validation failures, and downstream operational errors MUST be signaled using connectrpc.com/connect error constructors with explicit status codes.

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