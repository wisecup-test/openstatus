---
version: 2
name: cross-cutting-server-side-route-handlers-proxy-a0ac
description: "Adoption of next/server for Server-Side HTTP and Proxy Handling: Server Side Route Handlers Proxy Layers"
metadata:
  actual:
    adr_id: a0acb04e-c42e-41e8-9966-0707335d9f46
    status: accepted
    candidate_group_id: a8fe49ef-a92a-4310-8b4c-0d125ad33f95
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Adoption of next/server for Server-Side HTTP and Proxy Handling: Server Side Route Handlers Proxy Layers

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NXT-001** MUST: Server-side route handlers, proxy layers, and domain-resolution modules MUST use next/server abstractions for handling incoming HTTP requests, inspecting headers and query parameters, and generating HTTP responses.

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