---
version: 2
name: cross-cutting-route-handlers-isolate-domain-business-6fa1
description: "Adoption of next/server for Server Route and Request Handling: Route Handlers Isolate Domain Business Logic"
metadata:
  actual:
    adr_id: 6fa13c08-f2af-4229-86f6-f3982d6d88e3
    status: accepted
    candidate_group_id: 2dd81b33-c7f2-44a9-87ad-46fa4c700394
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/**
---

# Adoption of next/server for Server Route and Request Handling: Route Handlers Isolate Domain Business Logic

Scope: applies to `apps/dashboard/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ROUTE-001** SHOULD: Route handlers SHOULD isolate domain business logic and database interactions into designated internal service modules, keeping transport-level request extraction within the server module boundary.

### Verify

```bash
# Discover the project test runner and execute the server test suite
# Discover the project type checker and verify type compliance across server route handlers
# Discover the project linter and execute static analysis across server modules
```

**Accept when:**
- All server route handlers and proxy modules compile without type errors using the server module interfaces.
- HTTP endpoint test suites pass successfully, verifying header extraction and cookie lifecycle management.
- Static analysis confirms no unauthorized direct transport mutations or unsupported runtime socket access in server route handlers.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated testing and type verification pipelines, as well as peer reviews, enforce these standards.
</enforcement>