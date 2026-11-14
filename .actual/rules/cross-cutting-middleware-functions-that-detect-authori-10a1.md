---
version: 2
name: cross-cutting-middleware-functions-that-detect-authori-10a1
description: "Hono Framework Middleware and Context Management: Middleware Functions That Detect Authorization Failures"
metadata:
  actual:
    adr_id: 10a19cc9-9950-426c-9fdc-f35ac75ca695
    status: accepted
    candidate_group_id: 87aef6b0-858a-4df3-b448-91ee6ae3bf48
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
---

# Hono Framework Middleware and Context Management: Middleware Functions That Detect Authorization Failures

Scope: applies to `apps/server/src/libs/middlewares/**`.

These rules are ALWAYS ACTIVE for server HTTP middleware modules and route processing pipelines managing request lifecycle, authorization, and telemetry, and request-scoped state accessors resolving domain entities or client request headers.

### Rules

- **R-HONO-001** MUST: Middleware functions that detect authorization failures, plan mismatches, or invalid scopes MUST raise exceptions using Hono HTTP exception abstractions to ensure consistent error response contracts.

### Verify

```bash
# Discover the repository dependency manifest and execute the project type-checking script
# Discover and run the project test suite to verify middleware context propagation and HTTP exception handling
```

**Accept when:**
- Static type analysis completes without errors across all middleware modules utilizing Hono context getters and request accessors.
- Automated test suites verify that scoped entities and headers propagate through the middleware pipeline and produce expected HTTP responses.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>