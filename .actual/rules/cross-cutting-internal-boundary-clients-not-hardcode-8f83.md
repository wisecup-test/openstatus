---
version: 2
name: cross-cutting-internal-boundary-clients-not-hardcode-8f83
description: "@openstatus/api Internal API Client Boundary Integration: Internal Boundary Clients Not Hardcode Endpoint"
metadata:
  actual:
    adr_id: 8f83250a-bf88-4523-8900-834d46f60207
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

# @openstatus/api Internal API Client Boundary Integration: Internal Boundary Clients Not Hardcode Endpoint

Scope: applies to `apps/dashboard/src/components/onboarding/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all internal API client modules operating within server runtime environments and client-side UI components/hooks communicating with internal streaming or standard HTTP route handlers.

### Rules

- **R-INT-001** MUST_NOT: Internal API boundary clients MUST NOT hardcode endpoint URLs or bypass standardized transport header forwarders when communicating across service boundaries.

### Verify

```bash
# Discover and execute project test suite for internal API client boundaries
# Discover and execute project typecheck and linting suite for boundary contracts
# Discover and execute project integration test suite verifying session cookie propagation and streaming cancellation
echo "Discover and execute project test suite for internal API client boundaries"
echo "Discover and execute project typecheck and linting suite for boundary contracts"
echo "Discover and execute project integration test suite verifying session cookie propagation and streaming cancellation"
```

**Accept when:**
- All internal API client calls successfully execute and pass automated integration test suites without credential omission errors.
- Client-side streaming hooks successfully terminate active network requests upon component unmount without uncaught runtime errors.
- Repository type checking and linting pass with zero boundary contract violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All internal API integrations, transport configurations, and streaming hook implementations must comply with boundary rules and be verified via CI testing, static analysis, and peer code review.
</enforcement>