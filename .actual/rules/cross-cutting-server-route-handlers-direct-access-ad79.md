---
version: 2
name: cross-cutting-server-route-handlers-direct-access-ad79
description: "Adoption of next/headers for Server-Side Request Header Resolution: Server Route Handlers Direct Access Incoming"
metadata:
  actual:
    adr_id: ad7968cf-da6d-499c-a071-9ac4a7e22534
    status: accepted
    candidate_group_id: 0a7b592f-ca4b-43d4-a530-9faeef433969
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
      - src/lib/auth/**
---

# Adoption of next/headers for Server-Side Request Header Resolution: Server Route Handlers Direct Access Incoming

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NHD-001** MAY: Server route handlers with direct access to incoming request objects MAY read request headers directly from the request instance when next/headers store access is redundant.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute it to verify server route compilation and type conformance.
# Identify and run the repository automated test suite to confirm dynamic header inspection behaves as expected across route handlers and metadata endpoints.
```

**Accept when:**
- All server route handlers, metadata endpoints, and authentication utilities compile without type errors related to header access.
- Automated test suites pass with dynamic header retrieval functioning correctly for simulated forwarded and direct host headers.
- Static analysis checks pass with no invocation of header accessor functions within client components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>