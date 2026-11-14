---
version: 2
name: cross-cutting-server-side-internal-client-callers-68b4
description: "@openstatus/api Internal API Client Boundary Integration: Server Side Internal Client Callers Invoking"
metadata:
  actual:
    adr_id: 68b4f0b5-18bc-4ccf-89e1-6d45cedc14ed
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

# @openstatus/api Internal API Client Boundary Integration: Server Side Internal Client Callers Invoking

Scope: applies to `apps/dashboard/src/components/onboarding/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for internal API client modules operating within server runtime environments requiring session credential forwarding.

### Rules

- **R-INT-001** MUST: Server-side internal API client callers invoking the internal API through RPC clients MUST isolate client instantiation within server-only boundaries and forward incoming session cookie headers to preserve user authentication context across internal network boundaries.

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