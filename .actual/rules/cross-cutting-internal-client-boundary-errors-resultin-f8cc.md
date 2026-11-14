---
version: 2
name: cross-cutting-internal-client-boundary-errors-resultin-f8cc
description: "Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Client Boundary Errors Resulting Aborted"
metadata:
  actual:
    adr_id: f8cca4ef-0f53-41ae-8f86-05fdc43b1efe
    status: accepted
    candidate_group_id: 36c18984-4493-4cdc-afe4-8378a24b5a62
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/**
---

# Regional Checker Internal API Dispatch via fetch and AbortSignal: Internal Client Boundary Errors Resulting Aborted

Scope: applies to `apps/server/src/routes/**`.

These rules are ALWAYS ACTIVE for all internal service-to-service API client invocations dispatched from backend route handlers to regional execution worker nodes and remote procedure call handlers triggering remote execution tasks.

### Rules

- **R-INT-001** SHOULD: Internal client boundary errors resulting from aborted signals or failed HTTP responses SHOULD be logged with structured identifiers including workspace and entity identifiers.
- **R-INT-002** MANDATORY: Construct the signal parameter using AbortSignal.timeout configured with entity-specific timeout limits or system default limits for all internal fetch invocations.
- **R-INT-003** MANDATORY: Ensure internal authorization headers and regional routing headers are applied consistently across all route and handler dispatch points.

### Verify

```bash
# Discover the project test runner from repository configuration and run test suites covering internal client boundaries and route handlers
# Run static analysis and linting scripts to verify adherence to request timeout and cancellation signal policies
```

**Accept when:**
- All unit and integration test suites validating internal API requests pass with mocked or stubbed worker responses.
- Static verification confirms all internal fetch invocations supply a timeout signal and required boundary headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites and code reviews will block pull requests introducing unbounded fetch calls or missing regional routing headers.
</enforcement>