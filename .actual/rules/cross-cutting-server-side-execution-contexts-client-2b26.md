---
version: 2
name: cross-cutting-server-side-execution-contexts-client-2b26
description: "Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Server Side Execution Contexts Client Consume"
metadata:
  actual:
    adr_id: 2b26b5ce-13d4-466e-ba78-1360dc3f99c1
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

# Adoption of @openstatus/api Internal Module for Service Contracts and Procedure Routing: Server Side Execution Contexts Client Consume

Scope: applies to `apps/web/src/app/api/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-API-001** MUST: Server-side execution contexts and client-side UI contexts MUST consume procedure callers and error transformers exported by the dedicated subpath interfaces of the internal API module.

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