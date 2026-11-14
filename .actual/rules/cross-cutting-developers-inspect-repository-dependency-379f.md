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

- **R-NS-001** MUST: Developers MUST inspect the repository dependency lock artifact to determine the exact resolved version of next/server and verify that the after lifecycle API is supported prior to implementation.

### Verify

```bash
# Discover and execute the test runner execution script declared in the repository dependency manifest
# Discover and execute the project static analysis validation script
```

**Accept when:**
- Verification suites confirm that route handler responses dispatch without waiting for background persistence or analytics tracking tasks.
- Static analysis checks pass with zero unhandled promise rejections inside deferred post-response lifecycle callbacks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>