---
version: 2
name: cross-cutting-route-handlers-not-perform-blocking-512e
description: "next/server after Asynchronous Callback Adoption: Route Handlers Not Perform Blocking Analytical"
metadata:
  actual:
    adr_id: 512e5a06-3bf4-4c6a-a928-f58d337f3c2f
    status: accepted
    candidate_group_id: 7fce358c-dc15-43a3-bdca-b8edb4a18907
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# next/server after Asynchronous Callback Adoption: Route Handlers Not Perform Blocking Analytical

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for API route handlers processing requests that produce secondary telemetry, analytics, or asynchronous persistence, and stateless serverless endpoints prioritizing client-observed response latency over immediate side-effect confirmation.

### Rules

- **R-ROUTE-001** MUST_NOT: Route handlers MUST NOT perform blocking analytical telemetry or non-critical secondary cache writes prior to returning the HTTP response.

### Verify

```bash
# Discover the test runner execution script declared in the repository dependency manifest and run the test suite verifying route handler lifecycle behavior.
# Discover and execute the project static analysis validation script to verify that post-response background operations adhere to after callback scheduling constraints.
```

**Accept when:**
- Verification suites confirm that route handler responses dispatch without waiting for background persistence or analytics tracking tasks.
- Static analysis checks pass with zero unhandled promise rejections inside deferred post-response lifecycle callbacks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>