---
version: 2
name: cross-cutting-before-implementing-internal-client-depe-7769
description: "Regional Checker Internal API Dispatch via fetch and AbortSignal: Before Implementing Internal Client Dependencies That"
metadata:
  actual:
    adr_id: 7769f414-4f88-4912-b15c-17fae09ef371
    status: accepted
    candidate_group_id: 36c18984-4493-4cdc-afe4-8378a24b5a62
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Regional Checker Internal API Dispatch via fetch and AbortSignal: Before Implementing Internal Client Dependencies That

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all internal service-to-service API client invocations dispatched from backend route handlers to regional execution worker nodes, and remote procedure call handlers triggering remote execution tasks across internal infrastructure boundaries.

### Rules

- **R-INT-001** MUST: Before implementing internal client dependencies that rely on runtime or ecosystem libraries, the consumer MUST inspect the repository lock artifact and verify the exact resolved versions against official documentation.
- **R-INT-002** MUST: Construct the fetch signal parameter using AbortSignal.timeout configured with entity-specific timeout limits or system default limits.
- **R-INT-003** MUST: Ensure internal authorization headers and regional routing headers are applied consistently across all route and handler dispatch points.

### Verify

```bash
# Discover the project test runner from the repository configuration and execute the test suites covering internal client boundaries and route handlers.
# Discover the repository static analysis and linting scripts to verify adherence to request timeout and cancellation signal policies.
```

**Accept when:**
- All unit and integration test suites validating internal API requests pass with mocked or stubbed worker responses.
- Static verification confirms all internal fetch invocations supply a timeout signal and required boundary headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All internal API dispatch invocations must be validated via automated tests and static analysis.
</enforcement>