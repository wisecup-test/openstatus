---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-resolu-15ee
description: "Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Consumers Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 15ee9da0-2f69-4c64-a256-a0061e9fd0af
    status: accepted
    candidate_group_id: 20d6c0b5-4c6c-4f44-8245-265a2917445e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Consumers Inspect Repository Lock Resolution Artifact

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for HTTP route definitions, sub-routers, and workflow trigger endpoints exposed at service boundaries, and boundary endpoints requiring pre-flight secret authentication or contextual telemetry enrichment.

### Rules

- **R-HONO-001** MUST: Consumers MUST inspect the repository lock or resolution artifact to determine the exact resolved version of the web framework before implementing or modifying middleware interceptors.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify middleware registration and rule compliance across route entry points.
# Discover and run the project automated test suite to verify that boundary middleware correctly rejects unauthorized requests and populates telemetry context.
```

**Accept when:**
- All service endpoint test suites pass with middleware intercepting and rejecting unauthorized requests with a 401 status.
- Static verification confirms that perimeter authentication and request context tracking are declared via route middleware bindings rather than duplicated inside individual endpoint handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification of static analysis, linting, and automated integration/contract tests is mandatory.
</enforcement>