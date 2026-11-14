---
version: 2
name: cross-cutting-server-side-wrappers-instantiating-trpc-dbed
description: "Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Server Side Wrappers Instantiating Trpc Client"
metadata:
  actual:
    adr_id: dbedb0ee-b083-4a66-8593-7025592a2b7e
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

# Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Server Side Wrappers Instantiating Trpc Client

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Server-side wrappers instantiating @trpc/client MUST explicitly forward authentication cookies and headers across client boundaries while restricting execution to server environments.

### Verify

```bash
# Discover workspace verification script in repository manifest and execute type checking
# Locate repository testing script and execute test suite
# Inspect workspace linting scripts and execute static analysis
```

**Accept when:**
- Static type analysis completes with zero errors across all client remote procedure invocation points.
- All automated integration and component tests exercising client queries and mutations execute successfully.
- Code review confirms all new client data fetching and mutation operations route through the standardized remote procedure client.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline and peer code review enforce compliance.
</enforcement>