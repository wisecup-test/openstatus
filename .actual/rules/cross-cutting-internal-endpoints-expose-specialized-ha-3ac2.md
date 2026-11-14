---
version: 2
name: cross-cutting-internal-endpoints-expose-specialized-ha-3ac2
description: "Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Internal Endpoints Expose Specialized Handler Adapters"
metadata:
  actual:
    adr_id: 3ac23041-08ba-45ce-977c-cbe840584f2f
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

# Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Internal Endpoints Expose Specialized Handler Adapters

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MAY: Internal endpoints MAY expose specialized handler adapters when integrating with platform-specific execution environments as long as underlying procedure schemas originate from the internal API module.

### Verify

```bash
# Discover and run the project static analysis and type checking script to verify that all procedure calls match module contracts.
# Discover and run the workspace test suite to ensure server route handlers and client callers integrate without contract violations.
```

**Accept when:**
- All workspace type checks pass with zero signature or contract mismatch errors across all consuming modules.
- All route integration tests pass successfully without runtime errors during procedure invocation or error serialization.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>