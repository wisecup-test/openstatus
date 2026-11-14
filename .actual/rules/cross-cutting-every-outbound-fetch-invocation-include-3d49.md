---
version: 2
name: cross-cutting-every-outbound-fetch-invocation-include-3d49
description: "Fetch API for Outbound External Client Integrations: Every Outbound Fetch Invocation Include Abortsignal"
metadata:
  actual:
    adr_id: 3d4962ce-694d-4245-820b-82469bf93277
    status: accepted
    candidate_group_id: a984bfff-2085-4149-9bfa-0170b11bd480
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/lib/**
      - src/router/**
---

# Fetch API for Outbound External Client Integrations: Every Outbound Fetch Invocation Include Abortsignal

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all outbound HTTP and HTTPS network requests originating from API procedures, routers, and internal integration modules, including direct communication with external services, webhook receivers, and internal worker endpoints.

### Rules

- **R-FETCH-001** MUST: Every outbound fetch invocation MUST include an AbortSignal timeout configuration to guarantee deterministic connection termination.
- **R-FETCH-002** MUST: Developers MUST NOT omit timeout configurations on fetch calls.
- **R-FETCH-003** MUST: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-FETCH-004** MUST: Before writing code that uses a versioned library, execute the lock-version grounding steps in order (find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm API existence, re-run per dependency at point of use).

### Verify

```bash
# Discover the repository test runner from the root project manifest and execute integration tests covering outbound client boundaries.
# Discover the workspace static analysis script and verify all fetch call sites provide signal and authorization parameters.
```

**Accept when:**
- All outbound HTTP requests across API modules execute using the native fetch API with valid authorization and timeout signal options.
- Integration test suites mocking external endpoints pass without protocol or unhandled rejection errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>