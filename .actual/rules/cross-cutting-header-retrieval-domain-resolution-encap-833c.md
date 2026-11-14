---
version: 2
name: cross-cutting-header-retrieval-domain-resolution-encap-833c
description: "Adoption of next/headers for Server-Side Request Header Resolution: Header Retrieval Domain Resolution Encapsulate Store"
metadata:
  actual:
    adr_id: 833cff5f-3037-4832-809f-50982376bc33
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

# Adoption of next/headers for Server-Side Request Header Resolution: Header Retrieval Domain Resolution Encapsulate Store

Scope: applies to `apps/status-page/src/app/**`, `apps/status-page/src/lib/auth/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HDR-001** SHOULD: Header retrieval for domain resolution SHOULD encapsulate header store access in dedicated utility functions to ensure consistent fallback handling across entry points.
- **R-HDR-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-HDR-003** MANDATORY: Before writing code that uses a versioned library, execute in order:
  1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
  2. Identify the build tool from the manifest.
  3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
  4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
  5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
  6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- **R-HDR-004** MANDATORY: Encapsulate incoming host and protocol header resolution within shared domain helper functions to prevent duplicated header inspection logic.
- **R-HDR-005** MANDATORY: Verify that all endpoints invoking header access functions operate within server-side dynamic request boundaries to avoid runtime errors during build-time rendering.

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
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline and peer code review enforce compliance.
</enforcement>