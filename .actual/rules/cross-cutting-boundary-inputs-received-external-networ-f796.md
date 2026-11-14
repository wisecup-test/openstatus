---
version: 2
name: cross-cutting-boundary-inputs-received-external-networ-f796
description: "Zod Schema Definition and Runtime Validation Library Adoption: Boundary Inputs Received External Network Requests"
metadata:
  actual:
    adr_id: f79613b6-9bd3-4c68-b493-fe3495736a0f
    status: accepted
    candidate_group_id: c1f28a38-ae70-4493-874a-5738669cad24
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Zod Schema Definition and Runtime Validation Library Adoption: Boundary Inputs Received External Network Requests

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Boundary inputs received from external network requests, protocol adapters, and tool execution invocations MUST be validated using Zod schema parse or safeParse methods prior to downstream processing.

### Verify

```bash
# Discover and execute the project test runner script defined in the repository dependency manifest to verify schema validation test suites pass.
# Discover and execute the static type verification script declared in the repository workspace configuration to ensure all schema-inferred types compile without errors.
# Discover and execute the project linter and format validation scripts to verify adherence to validation standards across all route and adapter files.
```

**Accept when:**
- All schema validation test suites pass successfully across boundary adapters and route handlers.
- Static type analysis passes with zero diagnostic errors across all modules defining or consuming schema-inferred types.
- Runtime input parsing reliably rejects malformed boundary payloads and permits valid payloads across all integration entry points.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>