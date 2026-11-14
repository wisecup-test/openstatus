---
version: 2
name: cross-cutting-before-implementing-updating-trpc-client-f8f3
description: "Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Before Implementing Updating Trpc Client Integrations"
metadata:
  actual:
    adr_id: f8f3212e-b52a-4849-b94c-4d9d287851a2
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

# Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Before Implementing Updating Trpc Client Integrations

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Before implementing or updating @trpc/client integrations, consumers MUST locate the project dependency manifest, inspect the authoritative repository lock artifact to resolve the exact locked version, and verify all invoked APIs against that version's official documentation.

### Verify

```bash
# Discover the workspace verification script in the repository manifest and execute type checking across all client and API packages.
# Locate the repository testing script from the configuration files and execute the test suite to validate client query and mutation execution.
# Inspect the workspace linting scripts and execute static analysis to verify that no direct untyped network client calls bypass the remote procedure layer.
```

**Accept when:**
- Static type analysis completes with zero errors across all client remote procedure invocation points.
- All automated integration and component tests exercising client queries and mutations execute successfully.
- Code review confirms all new client data fetching and mutation operations route through the standardized remote procedure client.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>