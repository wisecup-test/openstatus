---
version: 2
name: cross-cutting-outbound-http-communications-across-serv-6987
description: "Fetch API for Outbound External Client Integrations: Outbound Http Communications Across Service Integration"
metadata:
  actual:
    adr_id: 6987408c-ea9e-4ea2-8f65-40dee2ad1428
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

# Fetch API for Outbound External Client Integrations: Outbound Http Communications Across Service Integration

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OUT-001** MUST: Outbound HTTP communications across service and integration boundaries MUST execute using the global fetch API rather than importing third-party transport libraries.

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