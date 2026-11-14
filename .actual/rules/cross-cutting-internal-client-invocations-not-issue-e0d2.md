---
version: 2
name: cross-cutting-internal-client-invocations-not-issue-e0d2
description: "Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Client Invocations Not Issue Unbounded"
metadata:
  actual:
    adr_id: e0d22d62-82dd-4db3-a3b3-83fe03d89e47
    status: accepted
    candidate_group_id: 36c18984-4493-4cdc-afe4-8378a24b5a62
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Client Invocations Not Issue Unbounded

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all internal service-to-service API client invocations dispatched from backend route handlers to regional execution worker nodes, and remote procedure call handlers triggering remote execution tasks across internal infrastructure boundaries.

### Rules

- **R-INT-001** MUST_NOT: Internal client invocations MUST NOT issue unbounded network requests without an attached cancellation signal.

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