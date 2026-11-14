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

These rules are ALWAYS ACTIVE for all client components, interactive forms, data tables, and server-side client proxy utilities invoking remote procedures.

### Rules

- **R-TRPC-001** MUST: Before implementing or updating @trpc/client integrations, consumers MUST locate the project dependency manifest, inspect the authoritative repository lock artifact to resolve the exact locked version, and verify all invoked APIs against that version's official documentation.

### Verify

```bash
# Discover workspace verification and test scripts from the repository manifest and execute type checking, tests, and linting
npm run typecheck
npm run test
npm run lint
```

**Accept when:**
- Static type analysis completes with zero errors across all client remote procedure invocation points.
- All automated integration and component tests exercising client queries and mutations execute successfully.
- Code review confirms all new client data fetching and mutation operations route through the standardized remote procedure client.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes must pass static type checking, automated tests, and lint checks.
</enforcement>