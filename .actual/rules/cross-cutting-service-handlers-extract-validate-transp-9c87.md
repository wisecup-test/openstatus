---
version: 2
name: cross-cutting-service-handlers-extract-validate-transp-9c87
description: "Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Service Handlers Extract Validate Transport Metadata"
metadata:
  actual:
    adr_id: 9c874ff5-f46a-4501-9d28-5b7ed6bf1c62
    status: accepted
    candidate_group_id: a5f353ac-efb9-4b32-b33a-11444dec2ad5
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Service Handlers Extract Validate Transport Metadata

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for private location RPC ingestion service endpoints and server handlers receiving probe data and metrics.

### Rules

- **R-ING-001** MUST: Service handlers MUST extract and validate transport metadata and authentication tokens from the request header container before proceeding with payload processing.

### Verify

```bash
# Discover and run the project test suite for all ingestion endpoint handlers
# Discover and run the project code analysis linter to verify RPC handler signatures conform to connectrpc.com/connect
```

**Accept when:**
- All ingestion endpoint tests pass successfully and validate protocol compliance.
- Static analysis confirms handler signatures adhere to connectrpc.com/connect request and response wrappers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites and peer code reviews verify compliance; non-conforming handlers are blocked from merging.
</enforcement>