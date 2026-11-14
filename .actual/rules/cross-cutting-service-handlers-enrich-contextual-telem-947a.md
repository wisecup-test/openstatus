---
version: 2
name: cross-cutting-service-handlers-enrich-contextual-telem-947a
description: "Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Service Handlers Enrich Contextual Telemetry Monitor"
metadata:
  actual:
    adr_id: 947a1dd3-cdaf-4fed-8270-108d9df4934b
    status: accepted
    candidate_group_id: a5f353ac-efb9-4b32-b33a-11444dec2ad5
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of connectrpc.com/connect for Service Ingestion Endpoints: Service Handlers Enrich Contextual Telemetry Monitor

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONN-001** SHOULD: Service handlers SHOULD enrich contextual telemetry with monitor and workspace metadata prior to downstream data forwarding.

### Verify

```bash
# Discover and run the project test execution harness for all ingestion endpoint handlers
# Discover and run the project code analysis linter to verify RPC handlers conform to ConnectRPC handler signatures
```

**Accept when:**
- All ingestion endpoint tests pass successfully and validate protocol compliance.
- Static analysis confirms handler signatures adhere to connectrpc.com/connect request and response wrappers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All RPC endpoint handlers must follow the consistent execution order (authentication verification, payload validation, context enrichment, downstream ingestion dispatch, and status response construction) and verify resolved library versions from the repository lock artifact before use.
</enforcement>