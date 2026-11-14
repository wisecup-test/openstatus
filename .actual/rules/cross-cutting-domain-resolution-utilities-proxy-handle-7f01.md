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

- **R-NEXT-001** MAY: Domain resolution utilities and proxy handlers MAY access incoming request metadata directly via next/server request wrappers when determining tenant routing context.

### Verify

```bash
# Discover the repository dependency manifest and run the declared type checking script to verify next/server import compliance.
# Discover and execute the repository test runner script targeting route handlers and proxy modules to validate server response contracts.
# Discover and run the project linting script to ensure no unauthorized HTTP response primitives are imported in server-side entry points.
```

**Accept when:**
- All server route handlers and proxy modules compile successfully using next/server types without type errors.
- Automated test suites for route handlers, badges, and proxy redirection pass with zero regressions.
- No deprecated or unsupported server-side response methods are invoked across server endpoints.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>