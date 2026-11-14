---
version: 2
name: cross-cutting-system-route-remote-procedure-calls-a2c7
description: "Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: System Route Remote Procedure Calls Error"
metadata:
  actual:
    adr_id: a2c7cd65-9323-4930-aad6-661e7df7e9e5
    status: accepted
    candidate_group_id: 6a007d6a-909a-425c-8c5a-15faed6eeac3
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/**
      - src/trpc/**
---

# Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: System Route Remote Procedure Calls Error

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OC-001** MUST: The system MUST route all remote procedure calls, error structures, and API service contracts through the @openstatus/api internal module across all application execution boundaries.

### Verify

```bash
# Discover and run the project static analysis, type checking, and test suite commands from the repository configuration.
```

**Accept when:**
- All workspace type checks pass with zero signature or contract mismatch errors across all consuming modules.
- All route integration tests pass successfully without runtime errors during procedure invocation or error serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>