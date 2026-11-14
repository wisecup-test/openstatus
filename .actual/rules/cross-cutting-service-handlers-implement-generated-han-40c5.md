---
version: 2
name: cross-cutting-service-handlers-implement-generated-han-40c5
description: "Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Service Handlers Implement Generated Handler Contract"
metadata:
  actual:
    adr_id: 40c567d1-d7f2-4299-88f1-5067cbf7273e
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

# Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Service Handlers Implement Generated Handler Contract

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/assertions/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all remote procedure call service contract definitions, client invocations, and telemetry ingestion endpoints across distributed probe components.

### Rules

- **R-CONNECT-001** MUST: All service handlers MUST implement the generated service handler contract interfaces for remote procedure calls.

### Verify

```bash
# Discover and run the project static analysis, linting scripts, and test suite from the repository configuration.
```

**Accept when:**
- All remote service client invocations instantiate the adopted RPC framework client without runtime or compilation errors.
- The project test suite passes with all unary service contract interfaces implemented and verified.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks executing compiler verification and test suites, and peer code review auditing service client integration points.
</enforcement>