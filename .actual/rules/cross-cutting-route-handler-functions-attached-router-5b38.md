---
version: 2
name: cross-cutting-route-handler-functions-attached-router-5b38
description: "Gin Framework for Service Endpoint Definitions: Route Handler Functions Attached Router Receive"
metadata:
  actual:
    adr_id: 5b38d6a1-7b2d-4234-918d-67734dce7bdc
    status: accepted
    candidate_group_id: f9f74977-16fb-4da2-9ee2-4a479df0625e
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Gin Framework for Service Endpoint Definitions: Route Handler Functions Attached Router Receive

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST: Route handler functions attached to the router MUST receive the framework context object and return structured JSON responses using the framework serialization primitives.

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