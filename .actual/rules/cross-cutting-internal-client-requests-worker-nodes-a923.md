---
version: 2
name: cross-cutting-internal-client-requests-worker-nodes-a923
description: "Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Client Requests Worker Nodes Supply"
metadata:
  actual:
    adr_id: a923b452-b460-4125-b800-2eefb7f3e5ee
    status: accepted
    candidate_group_id: 36c18984-4493-4cdc-afe4-8378a24b5a62
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Client Requests Worker Nodes Supply

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-INT-001** MUST: All internal API client requests to worker nodes MUST supply an AbortSignal generated via AbortSignal.timeout to enforce a hard deadline on request duration.

### Verify

```bash
# Discover and run the project test runner covering internal client boundaries and route handlers
# Discover and run repository static analysis and linting scripts to verify adherence to request timeout and cancellation signal policies
```

**Accept when:**
- All unit and integration test suites validating internal API requests pass with mocked or stubbed worker responses.
- Static verification confirms all internal fetch invocations supply a timeout signal and required boundary headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All internal API client requests to worker nodes must be verified for AbortSignal.timeout compliance and required headers.
</enforcement>