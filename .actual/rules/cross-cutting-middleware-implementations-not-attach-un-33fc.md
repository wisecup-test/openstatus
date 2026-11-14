---
version: 2
name: cross-cutting-middleware-implementations-not-attach-un-33fc
description: "Hono Framework Middleware and Context Management: Middleware Implementations Not Attach Unstructured Custom"
metadata:
  actual:
    adr_id: 33fc2826-b1f9-4f43-b2a0-ee6cc367927f
    status: accepted
    candidate_group_id: 87aef6b0-858a-4df3-b448-91ee6ae3bf48
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
---

# Hono Framework Middleware and Context Management: Middleware Implementations Not Attach Unstructured Custom

Scope: applies to `apps/server/src/libs/middlewares/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST_NOT: Middleware implementations MUST NOT attach unstructured custom properties directly to runtime execution globals outside the managed Hono context boundary.

### Verify

```bash
# Discover the repository dependency manifest and execute the project type-checking script
# Discover and run the project test suite to verify middleware context propagation
```

**Accept when:**
- Static type analysis completes without errors across all middleware modules utilizing Hono context getters and request accessors.
- Automated test suites verify that scoped entities and headers propagate through the middleware pipeline and produce expected HTTP responses.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>