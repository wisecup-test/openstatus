---
version: 2
name: cross-cutting-raw-http-request-headers-required-abdd
description: "Hono Framework Middleware and Context Management: Raw Http Request Headers Required Telemetry"
metadata:
  actual:
    adr_id: abdd6907-6e0b-4cc8-93fe-99c8adce1704
    status: accepted
    candidate_group_id: 87aef6b0-858a-4df3-b448-91ee6ae3bf48
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/libs/middlewares/**
---

# Hono Framework Middleware and Context Management: Raw Http Request Headers Required Telemetry

Scope: applies to `apps/server/src/libs/middlewares/**`.

These rules are ALWAYS ACTIVE for server HTTP middleware modules and route processing pipelines managing request lifecycle, authorization, telemetry, and request-scoped state accessors.

### Rules

- **R-HONO-001** SHOULD: Raw HTTP request headers required for telemetry, origin tracking, or user-agent detection SHOULD be accessed exclusively through the Hono request abstraction on the active context.

### Verify

```bash
# Discover repository dependency manifest and execute type-checking
# Discover and run the project test suite to verify middleware context propagation
```

**Accept when:**
- Static type analysis completes without errors across all middleware modules utilizing Hono context getters and request accessors.
- Automated test suites verify that scoped entities and headers propagate through the middleware pipeline and produce expected HTTP responses.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>