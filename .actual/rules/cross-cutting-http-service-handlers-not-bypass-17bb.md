---
version: 2
name: cross-cutting-http-service-handlers-not-bypass-17bb
description: "Gin Framework for Service Endpoint Definitions: Http Service Handlers Not Bypass Framework"
metadata:
  actual:
    adr_id: 17bb2f5c-a64d-4cff-84be-54dd856793fc
    status: accepted
    candidate_group_id: f9f74977-16fb-4da2-9ee2-4a479df0625e
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Gin Framework for Service Endpoint Definitions: Http Service Handlers Not Bypass Framework

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST_NOT: HTTP service handlers MUST_NOT bypass the framework context lifecycle to manipulate underlying network connections directly when servicing standard route handlers.

### Verify

```bash
# Discover and execute the project test runner to validate HTTP route handler responses
# Discover and execute the project static analysis suite to verify framework router compliance
```

**Accept when:**
- All discovered endpoint test suites pass with zero assertions failing on route responses.
- Static analysis validates that route handlers adhere to framework context usage conventions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>