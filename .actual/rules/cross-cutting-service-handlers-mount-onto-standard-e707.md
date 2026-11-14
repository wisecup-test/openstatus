---
version: 2
name: cross-cutting-service-handlers-mount-onto-standard-e707
description: "Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Service Handlers Mount Onto Standard Http"
metadata:
  actual:
    adr_id: e707f0c5-c0d1-430e-9efa-33787a19017f
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

# Connect RPC Protocol Adoption for External Service Contracts and Ingestion Endpoints: Service Handlers Mount Onto Standard Http

Scope: applies to `apps/private-location/internal/server/**`, `apps/private-location/proto/private_location/v1/**`.

These rules are ALWAYS ACTIVE for all external-facing remote procedure call interfaces connecting distributed agents to central ingestion endpoints, and server request handlers managing monitor synchronization and protocol telemetry ingestion.

### Rules

- **R-CONN-001** SHOULD: Service handlers SHOULD mount onto standard HTTP routers using the generated service handler constructor.
- **R-CONN-002** MANDATORY: Generate protocol service contracts and client stubs from centralized schema definitions before implementing handler functions.
- **R-CONN-003** MANDATORY: Extract caller authentication credentials from incoming protocol request headers at the handler boundary prior to invoking internal query handlers.
- **R-CONN-004** MANDATORY: Map all downstream storage and operational errors into structured protocol error codes to prevent leaking internal database failure details to external callers.
- **R-CONN-005** MANDATORY: Execute lock-version grounding before writing code that uses a versioned library (find dependency manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm API existence, re-run per dependency at point of use).

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
Claude Code MUST NOT skip or defer verification. All external-facing remote procedure call interfaces and ingestion endpoints must adhere strictly to generated service contracts, authenticated header extraction, and protocol error mapping. Continuous integration builds fail upon detecting untyped external endpoints or bypassed protocol request envelopes.
</enforcement>