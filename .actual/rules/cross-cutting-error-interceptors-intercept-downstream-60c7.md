---
version: 2
name: cross-cutting-error-interceptors-intercept-downstream-60c7
description: "@connectrpc/connect Interceptor Pipeline Architecture: Error Interceptors Intercept Downstream Procedure Errors"
metadata:
  actual:
    adr_id: 60c70aa5-db35-45f9-b836-f6696f708697
    status: accepted
    candidate_group_id: 4d15d7b7-58ac-4c28-ae26-f4b6181018d4
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/interceptors/**
---

# @connectrpc/connect Interceptor Pipeline Architecture: Error Interceptors Intercept Downstream Procedure Errors

Scope: applies to `apps/server/src/routes/rpc/interceptors/**`.

These rules are ALWAYS ACTIVE for all inbound and outbound RPC service procedures handled through the @connectrpc/connect server routing layer and cross-cutting request lifecycle handlers.

### Rules

- **R-CI-001** MUST: Error interceptors MUST intercept downstream procedure errors, differentiate recognized RPC server errors from unexpected runtime exceptions, and normalize diagnostic output before returning responses to the caller.

### Verify

```bash
# Discover and run the project static analysis and type checking suite to verify @connectrpc/connect interceptor type signatures
# Discover and execute the automated test runner to run unit and integration tests covering the RPC interceptor pipeline
```

**Accept when:**
- Static type analysis verifies that all interceptors strictly conform to @connectrpc/connect interceptor signatures without type errors
- Automated test suites covering RPC interceptors pass successfully, verifying that logging, error handling, and context propagation execute as specified

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated continuous integration checks and architectural peer review.
</enforcement>