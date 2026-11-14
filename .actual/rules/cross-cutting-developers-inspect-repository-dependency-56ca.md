---
version: 2
name: cross-cutting-developers-inspect-repository-dependency-56ca
description: "@openstatus/api Internal API Client Boundary Integration: Developers Inspect Repository Dependency Resolution Lock"
metadata:
  actual:
    adr_id: 56cac3c7-37e8-4565-bdd9-41956e1d6700
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

# @openstatus/api Internal API Client Boundary Integration: Developers Inspect Repository Dependency Resolution Lock

Scope: applies to `apps/dashboard/src/components/onboarding/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all internal API client modules operating within server runtime environments and client-side UI components/hooks communicating with internal streaming or standard HTTP route handlers.

### Rules

- **R-API-001** MUST: Developers MUST inspect the repository dependency resolution lock artifact to determine the exact installed version of internal API and client packages before implementing or modifying internal API boundary integrations.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>