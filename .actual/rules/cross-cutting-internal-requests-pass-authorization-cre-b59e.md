---
version: 2
name: cross-cutting-internal-requests-pass-authorization-cre-b59e
description: "Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Requests Pass Authorization Credentials Via"
metadata:
  actual:
    adr_id: b59ef2c5-a704-436d-a4a5-a86a50dcebbe
    status: accepted
    candidate_group_id: 36c18984-4493-4cdc-afe4-8378a24b5a62
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Requests Pass Authorization Credentials Via

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all internal service-to-service API client invocations dispatched from backend route handlers to regional execution worker nodes and remote procedure call handlers.

### Rules

- **R-INT-001** MUST: Internal API requests MUST pass internal authorization credentials via the Authorization header using environment-configured secrets.

### Verify

```bash
# Discover the project test runner from the repository configuration and execute the test suites covering internal client boundaries and route handlers.
# Discover the repository static analysis and linting scripts to verify adherence to request timeout and cancellation signal policies.
```

**Accept when:**
- All unit and integration test suites validating internal API requests pass with mocked or stubbed worker responses.
- Static verification confirms all internal fetch invocations supply a timeout signal and required boundary headers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>