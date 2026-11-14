---
version: 2
name: cross-cutting-service-handlers-not-write-raw-d5fc
description: "Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Service Handlers Not Write Raw Unformatted"
metadata:
  actual:
    adr_id: d5fcb6c0-ea16-4453-9c1a-ca7ffdb00b9b
    status: accepted
    candidate_group_id: a5f353ac-efb9-4b32-b33a-11444dec2ad5
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Service Handlers Not Write Raw Unformatted

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CON-001** MUST_NOT: Service handlers MUST NOT write raw unformatted HTTP responses or bypass the ConnectRPC response constructor when returning payloads to callers.

### Verify

```bash
# Discover and run the test suite for all ingestion endpoint handlers
# Discover and run the project code analysis linter to verify RPC handler signatures conform to ConnectRPC
```

**Accept when:**
- All ingestion endpoint tests pass successfully and validate protocol compliance.
- Static analysis confirms handler signatures adhere to connectrpc.com/connect request and response wrappers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>