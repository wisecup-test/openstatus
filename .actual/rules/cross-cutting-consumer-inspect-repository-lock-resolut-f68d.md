---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-resolut-f68d
description: "Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Consumer Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: f68d2b0b-83b3-47db-b817-d2e978c91bfc
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

# Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Consumer Inspect Repository Lock Resolution Artifact

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONN-001** MUST: The consumer MUST inspect the repository lock or resolution artifact to determine the exact resolved dependency version prior to implementing or modifying protocol contracts.
- **R-CONN-002** MUST: Generate protocol service contracts and client stubs from centralized schema definitions before implementing handler functions.
- **R-CONN-003** MUST: Extract caller authentication credentials from incoming protocol request headers at the handler boundary prior to invoking internal query handlers.
- **R-CONN-004** MUST: Map all downstream storage and operational errors into structured protocol error codes to prevent leaking internal database failure details to external callers.

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