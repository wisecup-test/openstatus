---
version: 2
name: cross-cutting-route-handlers-performing-non-critical-826a
description: "next/server after Asynchronous Callback Adoption: Route Handlers Performing Non Critical Post"
metadata:
  actual:
    adr_id: 826a44b4-2d88-4f7b-b9b9-cb4e4e433bf4
    status: accepted
    candidate_group_id: 7fce358c-dc15-43a3-bdca-b8edb4a18907
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# next/server after Asynchronous Callback Adoption: Route Handlers Performing Non Critical Post

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for API route handlers processing requests that produce secondary telemetry, analytics, or asynchronous persistence, and stateless serverless endpoints prioritizing client-observed response latency over immediate side-effect confirmation.

### Rules

- **R-ROUTE-001** MUST: Route handlers performing non-critical post-response side effects MUST schedule those operations using the after callback function from next/server rather than awaiting them on the primary response path.

### Verify

```bash
# Discover the test runner execution script declared in the repository dependency manifest and run the test suite verifying route handler lifecycle behavior.
# Discover and execute the project static analysis validation script to verify that post-response background operations adhere to after callback scheduling constraints.
```

**Accept when:**
- Verification suites confirm that route handler responses dispatch without waiting for background persistence or analytics tracking tasks.
- Static analysis checks pass with zero unhandled promise rejections inside deferred post-response lifecycle callbacks.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests with synchronous non-critical side effects blocking route responses will be blocked until refactored into deferred callbacks.
</enforcement>