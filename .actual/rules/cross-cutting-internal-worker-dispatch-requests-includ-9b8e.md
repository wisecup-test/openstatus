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
- **R-INT-002** MUST: Construct the signal parameter using AbortSignal.timeout configured with entity-specific timeout limits or system default limits.
- **R-INT-003** MUST: Ensure internal authorization headers and regional routing headers are applied consistently across all route and handler dispatch points.
- **R-INT-004** MUST: Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo. (2) Identify the build tool from the manifest. (3) Inspect the repository lock or resolution artifact to determine the exact resolved version. (4) Look up official documentation for that exact version. (5) Confirm every API, class, or function exists in that version's documentation. (6) Re-run steps 3-5 per dependency at point of use.

### Verify

```bash
# Discover the project test runner from the repository configuration and execute test suites covering internal client boundaries and route handlers
# Discover repository static analysis and linting scripts to verify adherence to request timeout and cancellation signal policies
```

**Accept when:**
- All unit and integration test suites validating internal API requests pass with mocked or stubbed worker responses.
- Static verification confirms all internal fetch invocations supply a timeout signal and required boundary headers.

<enforcement>
Claude Code MUST NOT skip or defer verification. All internal client invocations must be statically and dynamically verified for proper timeout and regional routing header compliance.
</enforcement>