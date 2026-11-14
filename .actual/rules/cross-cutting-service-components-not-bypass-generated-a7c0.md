---
version: 2
name: cross-cutting-service-components-not-bypass-generated-a7c0
description: "Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Service Components Not Bypass Generated Rpc"
metadata:
  actual:
    adr_id: a7c0fcea-e6d1-460d-9a1b-358e70de53d6
    status: accepted
    candidate_group_id: 96e6c8cc-2c78-4f76-b2f7-27ffc10a6a9c
    evidence:
      source: deep
  scope:
    root: apps/checker
    paths:
      - checker/**
      - pkg/assertions/**
      - proto/private_location/v1/**
---

# Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Service Components Not Bypass Generated Rpc

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/assertions/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all remote procedure call service contract definitions and client invocations across distributed probe components, as well as telemetry ingestion endpoints coordinating check results between runners and coordinators.

### Rules

- **R-CONN-001** MUST_NOT: Service components MUST NOT bypass the generated RPC client interfaces to perform ad-hoc transport calls for telemetry ingestion.

### Verify

```bash
# Discover and run project static analysis, linting scripts, and test suite to verify client and handler contract compliance across service boundaries
```

**Accept when:**
- All remote service client invocations instantiate the adopted RPC framework client without runtime or compilation errors.
- The project test suite passes with all unary service contract interfaces implemented and verified.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests bypassing generated RPC client interfaces are blocked until contracts are adopted.
</enforcement>