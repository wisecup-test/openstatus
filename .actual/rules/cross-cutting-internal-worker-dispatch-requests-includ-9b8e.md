---
version: 2
name: cross-cutting-internal-worker-dispatch-requests-includ-9b8e
description: "Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Worker Dispatch Requests Include Regional"
metadata:
  actual:
    adr_id: 9b8e7085-0620-4a19-9b3c-d217440aeaf4
    status: accepted
    candidate_group_id: 36c18984-4493-4cdc-afe4-8378a24b5a62
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Worker Dispatch Requests Include Regional

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all internal service-to-service API client invocations dispatched from backend route handlers to regional execution worker nodes and remote procedure call handlers triggering remote execution tasks across internal infrastructure boundaries.

### Rules

- **R-INT-001** MUST: Internal worker dispatch requests MUST include the regional affinity header specifying the target execution region alongside serialized JSON payloads.

### Verify

```bash
# Discover the project test runner from repository configuration and execute test suites covering internal client boundaries and route handlers
# Discover repository static analysis and linting scripts to verify adherence to request timeout and cancellation signal policies
```

**Accept when:**
- All unit and integration test suites validating internal API requests pass with mocked or stubbed worker responses.
- Static verification confirms all internal fetch invocations supply a timeout signal and required boundary headers.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>