---
version: 2
name: cross-cutting-rpc-handler-methods-accept-incoming-4e6b
description: "Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Rpc Handler Methods Accept Incoming Payloads"
metadata:
  actual:
    adr_id: 4e6b146e-9899-43e0-a8a4-639679ee0d74
    status: accepted
    candidate_group_id: 35c33ebb-f78e-4e67-899c-0be69ed767c1
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
      - proto/private_location/v1/**
---

# Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Rpc Handler Methods Accept Incoming Payloads

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-RPC-001** MUST: All RPC handler methods MUST accept incoming payloads encapsulated within typed protocol request containers and return responses encapsulated within typed protocol response containers.

### Verify

```bash
# Discover and execute the repository dependency lock verification script to validate locked protocol library versions.
# Discover and execute the repository test runner targeting all external protocol handler suites.
# Discover and execute the repository static analysis and linting suite to verify protocol contract compliance.
```

**Accept when:**
- All external RPC endpoints register handlers adhering to the generated service interface specifications.
- RPC handlers accept typed request containers, return typed response containers, and report errors using protocol status codes.
- Static contract and lock file verification checks pass without unauthenticated or untyped handler regressions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>