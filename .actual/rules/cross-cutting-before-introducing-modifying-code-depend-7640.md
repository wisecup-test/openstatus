---
version: 2
name: cross-cutting-before-introducing-modifying-code-depend-7640
description: "Adoption of next/headers for Server-Side Request Header Resolution: Before Introducing Modifying Code Depending Adopted"
metadata:
  actual:
    adr_id: 7640e7e1-a694-4487-ab13-d1f5969b8e74
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

# Adoption of next/headers for Server-Side Request Header Resolution: Before Introducing Modifying Code Depending Adopted

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Before introducing or modifying code depending on the adopted framework module, developers MUST inspect the repository dependency manifest and resolution lock artifact to verify the exact resolved version and align API usage with its official documentation.
- **R-ADR-002** MUST: Encapsulate incoming host and protocol header resolution within shared domain helper functions to prevent duplicated header inspection logic.
- **R-ADR-003** MUST: Verify that all endpoints invoking header access functions operate within server-side dynamic request boundaries to avoid runtime errors during build-time rendering.

### Verify

```bash
# Discover and run the project verification script from the repository manifest
# and execute automated test suites to confirm dynamic header inspection behaves as expected.
# (Derive exact commands from the repository package manager and configuration)
npm run test
```

**Accept when:**
- All server route handlers, metadata endpoints, and authentication utilities compile without type errors related to header access.
- Automated test suites pass with dynamic header retrieval functioning correctly for simulated forwarded and direct host headers.
- Static analysis checks pass with no invocation of header accessor functions within client components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>