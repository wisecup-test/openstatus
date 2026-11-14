---
version: 2
name: cross-cutting-client-modules-interacting-internal-boun-ee81
description: "@openstatus/api Internal API Client Boundary Integration: Client Modules Interacting Internal Boundaries Execute"
metadata:
  actual:
    adr_id: ee81835a-4ee1-4ddd-9054-d8667860d7d0
    status: accepted
    candidate_group_id: 1c383329-dd15-4c54-bd3d-525d24ca03a5
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/components/onboarding/**
      - src/lib/trpc/**
---

# @openstatus/api Internal API Client Boundary Integration: Client Modules Interacting Internal Boundaries Execute

Scope: applies to `apps/dashboard/src/components/onboarding/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-API-001** MUST: Client modules interacting with internal API boundaries MUST execute transport calls through explicit boundary configurations that isolate client credentials and connection lifecycles according to the executing runtime environment.

### Verify

```bash
echo "Discover and execute project test suite for internal API client boundaries"
echo "Discover and execute project typecheck and linting suite for boundary contracts"
echo "Discover and execute project integration test suite verifying session cookie propagation and streaming cancellation"
```

**Accept when:**
- All internal API client calls successfully execute and pass automated integration test suites without credential omission errors.
- Client-side streaming hooks successfully terminate active network requests upon component unmount without uncaught runtime errors.
- Repository type checking and linting pass with zero boundary contract violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>