---
version: 2
name: cross-cutting-external-service-credentials-including-b-01ec
description: "Fetch API for Outbound External Client Integrations: External Service Credentials Including Bearer Tokens"
metadata:
  actual:
    adr_id: 01ecc660-52a3-42ca-8d4f-73192ceee5d0
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

# Fetch API for Outbound External Client Integrations: External Service Credentials Including Bearer Tokens

Scope: applies to `packages/api/src/lib/**`, `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-EXT-001** MUST: External service credentials, including bearer tokens, basic authorization secrets, and webhook URLs, MUST be retrieved strictly from validated runtime environment configurations and passed via request headers or target URLs.

### Verify

```bash
# Discover the repository test runner from the root project manifest and execute integration tests covering outbound client boundaries.
# Discover the workspace static analysis script and verify all fetch call sites provide signal and authorization parameters.
```

**Accept when:**
- All outbound HTTP requests across API modules execute using the native fetch API with valid authorization and timeout signal options.
- Integration test suites mocking external endpoints pass without protocol or unhandled rejection errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification of outbound network requests using the global fetch API with configured abort signals and validated environment credentials is mandatory.
</enforcement>