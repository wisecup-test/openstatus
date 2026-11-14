---
version: 2
name: cross-cutting-route-handlers-server-side-middleware-966b
description: "Adoption of next/server for Server-Side HTTP and Proxy Handling: Route Handlers Server Side Middleware Not"
metadata:
  actual:
    adr_id: 966b6e01-cd3b-46c1-be73-0dd5d85ea212
    status: accepted
    candidate_group_id: a8fe49ef-a92a-4310-8b4c-0d125ad33f95
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of next/server for Server-Side HTTP and Proxy Handling: Route Handlers Server Side Middleware Not

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all server-side route handlers, API endpoints, proxy handlers, and domain resolution utilities within the status page application.

### Rules

- **R-NSV-001** MUST_NOT: Route handlers and server-side middleware MUST NOT instantiate alternative HTTP server abstractions or bypass next/server request and response primitives when responding across application boundaries.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>