---
version: 2
name: cross-cutting-when-consuming-runtime-library-dependenc-fd10
description: "Fetch API for Outbound External Client Integrations: When Consuming Runtime Library Dependencies That"
metadata:
  actual:
    adr_id: fd10d115-3231-41bc-9ff4-36ff15f67251
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

# Fetch API for Outbound External Client Integrations: When Consuming Runtime Library Dependencies That

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EXT-001** MUST: When consuming runtime or library dependencies that provide or polyfill fetch capabilities, the development team MUST inspect the repository dependency manifest and lock artifact to resolve the exact version before implementation.

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