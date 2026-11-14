---
version: 2
name: cross-cutting-operations-delegated-after-callback-rema-92e8
description: "next/server after Asynchronous Callback Adoption: Operations Delegated After Callback Remain Idempotent"
metadata:
  actual:
    adr_id: 92e8ae17-549e-46c8-8240-b22734e712e7
    status: accepted
    candidate_group_id: 7fce358c-dc15-43a3-bdca-b8edb4a18907
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# next/server after Asynchronous Callback Adoption: Operations Delegated After Callback Remain Idempotent

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for API route handlers processing incoming requests requiring immediate client responses alongside secondary side effects like telemetry and persistence.

### Rules

- **R-SERV-001** SHOULD: Operations delegated to the after callback SHOULD remain idempotent and bounded in execution time to prevent premature runtime termination.
- **R-SERV-002** MANDATORY: Wrap distinct secondary tasks inside the deferred execution callback in separate try-catch blocks to prevent an initial failure from terminating subsequent operations.
- **R-SERV-003** MANDATORY: Capture all request-scoped data into local constants before delegating to the deferred callback to prevent state corruption across asynchronous boundaries.

### Verify

```bash
# Discover and execute the test runner execution script declared in the repository dependency manifest
# to verify route handler lifecycle behavior and response completion timing.
# Discover and execute the project static analysis validation script to verify post-response background operations.
```

**Accept when:**
- Verification suites confirm that route handler responses dispatch without waiting for background persistence or analytics tracking tasks.
- Static analysis checks pass with zero unhandled promise rejections inside deferred post-response lifecycle callbacks.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated test suites, code review, and static analysis validate response completion timing and side-effect isolation.
</enforcement>