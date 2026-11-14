---
version: 2
name: cross-cutting-request-context-middleware-initialize-un-161a
description: "Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Request Context Middleware Initialize Unique Correlation"
metadata:
  actual:
    adr_id: 161a2078-94c0-458b-96f6-1fcc042819e2
    status: accepted
    candidate_group_id: 20d6c0b5-4c6c-4f44-8245-265a2917445e
    evidence:
      source: deep
  scope:
    root: apps/workflows
    paths:
      - src/**
---

# Hono Middleware Pipeline for Boundary Authorization and Context Telemetry: Request Context Middleware Initialize Unique Correlation

Scope: applies to `apps/workflows/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HNC-001** MUST: Request context middleware MUST initialize a unique correlation request identifier and contextual event state on the execution context before delegating to the next middleware in the pipeline.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify middleware registration and rule compliance across route entry points.
# Discover and run the project automated test suite to verify that boundary middleware correctly rejects unauthorized requests and populates telemetry context.
```

**Accept when:**
- All service endpoint test suites pass with middleware intercepting and rejecting unauthorized requests with a 401 status.
- Static verification confirms that perimeter authentication and request context tracking are declared via route middleware bindings rather than duplicated inside individual endpoint handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated integration and contract tests must verify unauthorized requests yield 401 responses, and static analysis/peer review must confirm route registration trees.
</enforcement>