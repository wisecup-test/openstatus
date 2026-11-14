---
version: 2
name: cross-cutting-internal-service-client-dispatches-regio-f47f
description: "Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Service Client Dispatches Regional Execution"
metadata:
  actual:
    adr_id: f47fa45c-edb1-45d9-9407-ed69b4f30ffa
    status: accepted
    candidate_group_id: 36c18984-4493-4cdc-afe4-8378a24b5a62
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Service Client Dispatches Regional Execution

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REG-001** MUST: Internal service client dispatches to regional execution workers MUST use the fetch client configured with explicit regional routing headers, internal authentication headers, and an AbortSignal timeout.

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