---
version: 2
name: cross-cutting-context-propagation-passed-through-unary-ab87
description: "Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Context Propagation Passed Through Unary Rpc"
metadata:
  actual:
    adr_id: ab879aa8-46fa-4e10-917e-b66555c9dd07
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

# Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Context Propagation Passed Through Unary Rpc

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/assertions/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONN-001** SHOULD: Context propagation SHOULD be passed through all unary RPC client calls to ensure proper request cancellation and timeout handling.

### Verify

```bash
# Discover and run the project static analysis and linting scripts from the repository configuration.
# Discover and run the test suite to verify client and handler contract compliance across service boundaries.
```

**Accept when:**
- All remote service client invocations instantiate the adopted RPC framework client without runtime or compilation errors.
- The project test suite passes with all unary service contract interfaces implemented and verified.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>