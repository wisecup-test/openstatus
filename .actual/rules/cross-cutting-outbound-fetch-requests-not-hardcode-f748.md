---
version: 2
name: cross-cutting-outbound-fetch-requests-not-hardcode-f748
description: "Fetch API for Outbound External Client Integrations: Outbound Fetch Requests Not Hardcode Authorization"
metadata:
  actual:
    adr_id: f7481180-c31d-43a9-86cd-f0e97285a7eb
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

# Fetch API for Outbound External Client Integrations: Outbound Fetch Requests Not Hardcode Authorization

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all outbound HTTP and HTTPS network requests originating from API procedures, routers, and internal integration modules, including direct communication with external services, webhook receivers, and internal worker endpoints.

### Rules

- **R-OCF-001** MUST_NOT: Outbound fetch requests MUST NOT hardcode authorization credentials, sensitive tokens, or target environment URLs within call sites.

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