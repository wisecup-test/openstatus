---
version: 2
name: cross-cutting-server-endpoints-evaluating-multi-tenant-7c5e
description: "Adoption of next/headers for Server-Side Request Header Resolution: Server Endpoints Evaluating Multi Tenant Forwarded"
metadata:
  actual:
    adr_id: 7c5e4dcb-7b2c-4ea1-8054-148091d582c1
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

# Adoption of next/headers for Server-Side Request Header Resolution: Server Endpoints Evaluating Multi Tenant Forwarded

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NXT-001** MUST: Server endpoints evaluating multi-tenant or forwarded routing MUST inspect header values retrieved via next/headers rather than assuming fixed runtime host configurations.

### Verify

```bash
# Discover the project verification script from the repository manifest and execute it to verify server route compilation and type conformance
npm run build
# Identify and run the repository automated test suite to confirm dynamic header inspection behaves as expected across route handlers and metadata endpoints
npm test
```

**Accept when:**
- All server route handlers, metadata endpoints, and authentication utilities compile without type errors related to header access.
- Automated test suites pass with dynamic header retrieval functioning correctly for simulated forwarded and direct host headers.
- Static analysis checks pass with no invocation of header accessor functions within client components.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline runs automated type checking and linting across all server modules, and peer code reviews enforce proper header resolution mechanisms.
</enforcement>