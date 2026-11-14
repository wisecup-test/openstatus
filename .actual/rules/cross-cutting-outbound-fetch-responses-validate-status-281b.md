---
version: 2
name: cross-cutting-outbound-fetch-responses-validate-status-281b
description: "Fetch API for Outbound External Client Integrations: Outbound Fetch Responses Validate Status Indicators"
metadata:
  actual:
    adr_id: 281b0900-f1d7-48f5-9283-793464877205
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

# Fetch API for Outbound External Client Integrations: Outbound Fetch Responses Validate Status Indicators

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OUT-001** SHOULD: Outbound fetch responses SHOULD validate status indicators and parse expected payload structures prior to returning data to domain logic.

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