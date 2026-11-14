---
version: 2
name: cross-cutting-services-adopting-gin-routing-framework-3abb
description: "Gin Framework for Service Endpoint Definitions: Services Adopting Gin Routing Framework Boundary"
metadata:
  actual:
    adr_id: 3abb21cf-2d26-4f95-ab87-10b7c0376065
    status: accepted
    candidate_group_id: f9f74977-16fb-4da2-9ee2-4a479df0625e
    evidence:
      source: deep
  scope:
    root: apps/railway-proxy
---

# Gin Framework for Service Endpoint Definitions: Services Adopting Gin Routing Framework Boundary

Scope: applies to `apps/railway-proxy/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-GIN-001** MUST: Services adopting the Gin routing framework for boundary definitions MUST register HTTP endpoint handlers through the Gin router engine rather than introducing disparate HTTP server abstractions within the same service boundary.

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