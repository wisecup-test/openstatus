---
version: 2
name: cross-cutting-service-boundary-definitions-extract-cus-18ac
description: "Gin Framework for Service Endpoint Definitions: Service Boundary Definitions Extract Custom Routing"
metadata:
  actual:
    adr_id: 18ac33e7-f981-421f-af4e-714e421bda16
    status: accepted
    candidate_group_id: f9f74977-16fb-4da2-9ee2-4a479df0625e
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Gin Framework for Service Endpoint Definitions: Service Boundary Definitions Extract Custom Routing

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** SHOULD: Service boundary definitions SHOULD extract custom routing headers from incoming request contexts through the framework context accessors before delegating downstream requests.

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