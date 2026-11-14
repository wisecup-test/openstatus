---
version: 2
name: cross-cutting-before-implementing-modifying-service-ha-4ce0
description: "Adoption of Connect RPC for Service API Implementation: Before Implementing Modifying Service Handlers That"
metadata:
  actual:
    adr_id: 4ce0c76c-9a1b-4d3e-b17d-14c4a179d975
    status: accepted
    candidate_group_id: 12986db6-19ad-4261-b7b0-f7f63637c6f9
    evidence:
      source: deep
  scope:
    root: apps/private-location
    paths:
      - internal/server/**
---

# Adoption of Connect RPC for Service API Implementation: Before Implementing Modifying Service Handlers That

Scope: applies to `apps/private-location/internal/server/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CONNECT-001** MUST: Before implementing or modifying service handlers that consume the Connect RPC library, engineers MUST inspect the repository dependency manifest and lock artifact to resolve the exact dependency version and verify API compatibility against authoritative documentation.
- **R-CONNECT-002** MANDATORY: Execute lock-version grounding before writing code that uses a versioned library: find dependency manifest, identify build tool, inspect repository lock or resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run steps per dependency at point of use for version-sensitive behavior.
- **R-CONNECT-003** MANDATORY: Implementations must register generated service handler interfaces onto HTTP routing trees using the standard Connect router registration functions.
- **R-CONNECT-004** MANDATORY: Handlers spawning asynchronous background tasks that exceed the lifecycle of the incoming RPC must derive detached context boundaries to prevent unintended cancellation.

### Verify

```bash
# Discover and execute the project verification script from the repository configuration across all service handler packages
# Discover and run the project test runner to execute unit and contract integration tests validating Connect RPC handler endpoints
```

**Accept when:**
- All service handler functions compile successfully against resolved Connect RPC types and pass static analysis verification.
- All RPC endpoint test suites pass with zero failures, confirming valid response envelope construction and expected error status mapping for unauthenticated and internal failure conditions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, architectural reviews, and automated checks enforce Connect RPC generic envelopes, error conventions, and strict handler registration.
</enforcement>