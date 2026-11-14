---
version: 2
name: cross-cutting-stream-consumers-processing-incremental-21b4
description: "@openstatus/api Internal API Client Boundary Integration: Stream Consumers Processing Incremental Chunk Payloads"
metadata:
  actual:
    adr_id: 21b46654-f31a-4a1e-97eb-ba5bfe406061
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

# @openstatus/api Internal API Client Boundary Integration: Stream Consumers Processing Incremental Chunk Payloads

Scope: applies to `apps/dashboard/src/components/onboarding/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for internal API client modules operating within server runtime environments requiring session credential forwarding and client-side UI components and hooks communicating with internal streaming or standard HTTP route handlers.

### Rules

- **R-API-001** MUST: Stream consumers processing incremental chunk payloads from internal API endpoints MUST parse incoming chunks using structured JSON parsing with defensive error boundaries to capture malformed lines without crashing consumer UI state.

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