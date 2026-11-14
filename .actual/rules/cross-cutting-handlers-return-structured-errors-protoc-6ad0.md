---
version: 2
name: cross-cutting-handlers-return-structured-errors-protoc-6ad0
description: "Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Handlers Return Structured Errors Protocol Error"
metadata:
  actual:
    adr_id: 6ad048ed-fe02-42d2-8f85-58d5a0736b81
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

# Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Handlers Return Structured Errors Protocol Error

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONN-001** MUST: Handlers MUST return structured errors using protocol error constructors with explicit status codes rather than naked error values.
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