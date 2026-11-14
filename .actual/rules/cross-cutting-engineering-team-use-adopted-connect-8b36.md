---
version: 2
name: cross-cutting-engineering-team-use-adopted-connect-8b36
description: "Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Engineering Team Use Adopted Connect Rpc"
metadata:
  actual:
    adr_id: 8b36aee1-8b47-4e56-97f4-3af8db487f11
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

# Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Engineering Team Use Adopted Connect Rpc

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/assertions/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONNECT-001** MUST: The engineering team MUST use the adopted Connect RPC framework for defining and consuming unary network service interfaces across distributed monitoring boundaries.

### Verify

```bash
# Discover and run the project static analysis, linting scripts, and test suite from the repository configuration to verify client and handler contract compliance.
```

**Accept when:**
- All remote service client invocations instantiate the adopted RPC framework client without runtime or compilation errors.
- The project test suite passes with all unary service contract interfaces implemented and verified.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>