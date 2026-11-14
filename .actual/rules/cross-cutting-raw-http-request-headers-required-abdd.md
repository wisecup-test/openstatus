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

These rules are ALWAYS ACTIVE for server HTTP middleware modules, route processing pipelines, and request-scoped state accessors resolving domain entities or client request headers.

### Rules

- **R-HONO-001** SHOULD: Raw HTTP request headers required for telemetry, origin tracking, or user-agent detection SHOULD be accessed exclusively through the Hono request abstraction on the active context.

### Verify

```bash
# Discover the repository dependency manifest and execute the project type-checking script to confirm all Hono context accessors adhere to defined context type contracts.
# Discover and run the project test suite to verify that middleware context propagation and HTTP exception handling pass all automated test scenarios.
```

**Accept when:**
- Static type analysis completes without errors across all middleware modules utilizing Hono context getters and request accessors.
- Automated test suites verify that scoped entities and headers propagate through the middleware pipeline and produce expected HTTP responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. Direct request mutation or bypassing Hono context accessors will be rejected during review.
</enforcement>