---
version: 2
name: cross-cutting-domain-resolution-utilities-proxy-handle-7f01
description: "Adoption of next/server for Server-Side HTTP and Proxy Handling: Domain Resolution Utilities Proxy Handlers Access"
metadata:
  actual:
    adr_id: 7f01af2c-c824-4d34-8052-693370318a8c
    status: accepted
    candidate_group_id: a8fe49ef-a92a-4310-8b4c-0d125ad33f95
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of next/server for Server-Side HTTP and Proxy Handling: Domain Resolution Utilities Proxy Handlers Access

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all server-side route handlers, API endpoints, proxy handlers, and domain resolution utilities within the status page application.

### Rules

- **R-NET-001** MAY: Domain resolution utilities and proxy handlers MAY access incoming request metadata directly via next/server request wrappers when determining tenant routing context.
- **R-NET-002** MANDATORY: Ensure route handlers import server primitives exclusively from next/server and adhere to supported handler function signatures for HTTP methods.
- **R-NET-003** MANDATORY: Utilize next/server response helpers for status code assignment, header mutation, and cookie writing to ensure downstream proxy middleware compatibility.
- **R-NET-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect lock/resolution artifact for exact version, look up official version-specific documentation, confirm API existence, and re-verify per dependency at point of use.

### Verify

```bash
# Discover the repository dependency manifest and run type checking
# (Discover build/type-check script from project configuration, e.g., npm run typecheck or yarn tsc)

# Discover and execute the test runner targeting route handlers and proxy modules
# (Discover test script from project configuration)

# Discover and run project linting script to ensure no unauthorized HTTP response primitives are imported
# (Discover lint script from project configuration)
```

**Accept when:**
- All server route handlers and proxy modules compile successfully using next/server types without type errors.
- Automated test suites for route handlers, badges, and proxy redirection pass with zero regressions.
- No deprecated or unsupported server-side response methods are invoked across server endpoints.

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification.
</enforcement>