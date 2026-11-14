---
version: 2
name: cross-cutting-consumer-inspect-repository-dependency-r-4b7f
description: "Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Consumer Inspect Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: 4b7f4c4b-7940-47c2-9878-efefe129b155
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

# Adoption of Connect RPC (connectrpc.com/connect) for Service Communication: Consumer Inspect Repository Dependency Resolution Artifact

Scope: applies to `apps/checker/checker/**`, `apps/checker/pkg/assertions/**`, `apps/checker/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all remote procedure call service contract definitions, client invocations, and telemetry ingestion endpoints across distributed probe components.

### Rules

- **R-CONN-001** MUST: The consumer MUST inspect the repository dependency resolution artifact to determine the exact locked version of the RPC framework before using its APIs.

### Verify

```bash
# Discover and run project static analysis, linting, and test suites from repository configuration
```

**Accept when:**
- All remote service client invocations instantiate the adopted RPC framework client without runtime or compilation errors.
- The project test suite passes with all unary service contract interfaces implemented and verified.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests bypassing the generated RPC client interfaces are blocked until contracts are adopted.
</enforcement>