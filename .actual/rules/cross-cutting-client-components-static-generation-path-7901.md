---
version: 2
name: cross-cutting-client-components-static-generation-path-7901
description: "Adoption of next/headers for Server-Side Request Header Resolution: Client Components Static Generation Paths That"
metadata:
  actual:
    adr_id: 7901aeac-3f4c-4926-9d7b-f04693a68051
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

# Adoption of next/headers for Server-Side Request Header Resolution: Client Components Static Generation Paths That

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NXT-001** MUST_NOT: Client components or static generation paths that execute outside request contexts MUST NOT import or call next/headers functions.

### Verify

```bash
# Discover and run the project verification script from the repository manifest
# and run the automated test suite to verify server route compilation and type conformance.
```

**Accept when:**
- All server route handlers, metadata endpoints, and authentication utilities compile without type errors related to header access.
- Automated test suites pass with dynamic header retrieval functioning correctly for simulated forwarded and direct host headers.
- Static analysis checks pass with no invocation of header accessor functions within client components.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline and peer code review enforce these rules.
</enforcement>