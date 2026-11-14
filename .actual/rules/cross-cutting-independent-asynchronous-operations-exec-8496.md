---
version: 2
name: cross-cutting-independent-asynchronous-operations-exec-8496
description: "next/server after Asynchronous Callback Adoption: Independent Asynchronous Operations Executed Within After"
metadata:
  actual:
    adr_id: 84968704-9422-4bf2-9ae8-562f94ae6a14
    status: accepted
    candidate_group_id: 7fce358c-dc15-43a3-bdca-b8edb4a18907
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/**
---

# next/server after Asynchronous Callback Adoption: Independent Asynchronous Operations Executed Within After

Scope: applies to `apps/web/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AFTR-001** MUST: Independent asynchronous operations executed within the after callback MUST be wrapped in isolated error-handling blocks to prevent failures in one task from halting subsequent tasks.
- **R-AFTR-002** MANDATORY: Wrap distinct secondary tasks inside the deferred execution callback in separate try-catch blocks to prevent an initial failure from terminating subsequent operations.
- **R-AFTR-003** MANDATORY: Capture all request-scoped data into local constants before delegating to the deferred callback to prevent state corruption across asynchronous boundaries.

### Verify

```bash
# Discover and run the test suite verifying route handler lifecycle behavior using the repository's test runner script
npm test
# Discover and execute the project static analysis validation script to verify post-response background operations
npm run lint
```

**Accept when:**
- Verification suites confirm that route handler responses dispatch without waiting for background persistence or analytics tracking tasks.
- Static analysis checks pass with zero unhandled promise rejections inside deferred post-response lifecycle callbacks.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>