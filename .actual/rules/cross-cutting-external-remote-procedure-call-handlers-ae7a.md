---
version: 2
name: cross-cutting-external-remote-procedure-call-handlers-ae7a
description: "Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: External Remote Procedure Call Handlers Implement"
metadata:
  actual:
    adr_id: ae7a1684-885f-4d55-9acf-23243014ccde
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

# Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: External Remote Procedure Call Handlers Implement

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EXT-001** MUST: External remote procedure call handlers MUST implement service contracts using Connect RPC (`connectrpc.com/connect`) for client and server protocol boundaries.

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