---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-379f
description: "next/server after Asynchronous Callback Adoption: Developers Inspect Repository Dependency Lock Artifact"
metadata:
  actual:
    adr_id: 379fc866-c391-46b6-a1c0-1993d80719c9
    status: accepted
    candidate_group_id: 7fce358c-dc15-43a3-bdca-b8edb4a18907
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# next/server after Asynchronous Callback Adoption: Developers Inspect Repository Dependency Lock Artifact

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for API route handlers processing requests that produce secondary telemetry, analytics, or asynchronous persistence, and stateless serverless endpoints prioritizing client-observed response latency over immediate side-effect confirmation.

### Rules

- **R-DEV-001** MUST: Developers MUST inspect the repository dependency lock artifact to determine the exact resolved version of next/server and verify that the after lifecycle API is supported prior to implementation.

### Verify

```bash
# Discover the test runner execution script declared in the repository dependency manifest and run the test suite verifying route handler lifecycle behavior.
# Discover and execute the project static analysis validation script to verify that post-response background operations adhere to after callback scheduling constraints.
```

**Accept when:**
- Verification suites confirm that route handler responses dispatch without waiting for background persistence or analytics tracking tasks.
- Static analysis checks pass with zero unhandled promise rejections inside deferred post-response lifecycle callbacks.

<enforcement>
Verification suites and peer code review must validate response completion timing and side-effect isolation. Pull requests with synchronous non-critical side effects blocking route responses will be blocked until refactored into deferred callbacks.
</enforcement>