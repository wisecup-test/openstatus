---
version: 2
name: cross-cutting-internal-requests-dispatched-client-envi-2dcc
description: "@openstatus/api Internal API Client Boundary Integration: Internal Requests Dispatched Client Environments Set"
metadata:
  actual:
    adr_id: 2dcc598c-4b82-4481-abb9-ffe64c863c5d
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

# @openstatus/api Internal API Client Boundary Integration: Internal Requests Dispatched Client Environments Set

Scope: applies to `apps/dashboard/src/components/onboarding/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for internal API client modules, client-side UI components, and hooks communicating with internal HTTP/streaming route handlers.

### Rules

- **R-INT-001** SHOULD: Internal API requests dispatched from client environments SHOULD set standard credentials inclusion to ensure session state is automatically propagated by the user agent.

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
Claude Code MUST NOT skip or defer verification. Compliance is enforced via automated continuous integration test suites, static analysis/lint rules restricting server-only modules from client bundle imports, and peer code reviews.
</enforcement>