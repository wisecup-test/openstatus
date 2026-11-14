---
version: 2
name: cross-cutting-frontend-components-client-side-form-5093
description: "Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Frontend Components Client Side Form Handlers"
metadata:
  actual:
    adr_id: 509339e7-99d1-4c08-a4d5-2fca29197f4b
    status: accepted
    candidate_group_id: ca020432-fbe7-4ec5-b2ca-43bb7b58d3ac
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/components/**
      - src/lib/trpc/**
---

# Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Frontend Components Client Side Form Handlers

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST_NOT: Frontend components and client-side form handlers MUST NOT issue raw untyped network requests for operations that have corresponding remote procedure definitions in the API layer.

### Verify

```bash
# Discover and run type checking across client and API packages via the workspace manifest
# Locate and run the repository testing suite
# Run repository static analysis / linting to check for unapproved direct untyped network client calls
```

**Accept when:**
- Static type analysis completes with zero errors across all client remote procedure invocation points.
- All automated integration and component tests exercising client queries and mutations execute successfully.
- Code review confirms all new client data fetching and mutation operations route through the standardized remote procedure client.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines and peer code reviews enforce compliance with these rules.
</enforcement>