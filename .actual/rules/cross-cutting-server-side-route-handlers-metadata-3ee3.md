---
version: 2
name: cross-cutting-server-side-route-handlers-metadata-3ee3
description: "Adoption of next/headers for Server-Side Request Header Resolution: Server Side Route Handlers Metadata Generators"
metadata:
  actual:
    adr_id: 3ee3974f-b1e3-4264-b39b-2bae560d16ac
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

# Adoption of next/headers for Server-Side Request Header Resolution: Server Side Route Handlers Metadata Generators

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for server-side API route handlers, metadata generation endpoints, and server-side authentication helper modules.

### Rules

- **R-NSH-001** MUST: Server-side route handlers, metadata generators, and authentication utilities requiring incoming HTTP header information MUST retrieve request headers using the next/headers module.

### Verify

```bash
# Discover and run the project verification script from the repository manifest
# Identify and run the repository automated test suite to confirm dynamic header inspection behaves as expected
```

**Accept when:**
- All server route handlers, metadata endpoints, and authentication utilities compile without type errors related to header access.
- Automated test suites pass with dynamic header retrieval functioning correctly for simulated forwarded and direct host headers.
- Static analysis checks pass with no invocation of header accessor functions within client components.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by continuous integration pipeline running automated type checking, linting, and peer code reviews.
</enforcement>