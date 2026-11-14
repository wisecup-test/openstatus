---
version: 2
name: cross-cutting-client-side-components-invoking-streamin-bc3d
description: "@openstatus/api Internal API Client Boundary Integration: Client Side Components Invoking Streaming Internal"
metadata:
  actual:
    adr_id: bc3d5dbc-fb14-4f95-afd2-71a18cd94c05
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

# @openstatus/api Internal API Client Boundary Integration: Client Side Components Invoking Streaming Internal

Scope: applies to `apps/dashboard/src/components/onboarding/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPENSTATUS-001** MUST: Client-side components invoking streaming internal API endpoints MUST supply an AbortSignal instance from an AbortController to guarantee deterministic cancellation and avoid orphaned background network consumption upon component unmount.

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