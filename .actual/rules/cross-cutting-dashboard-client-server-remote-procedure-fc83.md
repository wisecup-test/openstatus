---
version: 2
name: cross-cutting-dashboard-client-server-remote-procedure-fc83
description: "Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Dashboard Client Server Remote Procedure Communication"
metadata:
  actual:
    adr_id: fc83f0e8-a132-4176-9df8-9a427f164707
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

# Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Dashboard Client Server Remote Procedure Communication

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all dashboard client-to-server remote procedure communication, client-side components, forms, interactive views, and server-side client proxy utilities.

### Rules

- **R-TRPC-001** MUST: All dashboard client-to-server remote procedure communication MUST use @trpc/client as the standardized client communication layer bound to shared API router definitions.
- **R-TRPC-002** MANDATORY: Execute the Lock-Version Grounding sequence (find manifest, identify build tool, inspect lock/resolution artifact, look up official documentation for that exact version, confirm APIs exist, re-run per dependency at point of use) before writing code that uses a versioned library.
- **R-TRPC-003** MANDATORY: Strictly isolate server client utilities using server-only environment boundary markers and lint rules to prevent client-side bundling of server-specific dependencies.
- **R-TRPC-004** MANDATORY: Enforce centralized error handling and logging hooks in shared client configuration links to prevent unhandled RPC exceptions from leaking internal server details or breaking client UI state.

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
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines run automated type checking and test suites on pull requests, and peer code reviews verify compliance with client architecture and the absence of untyped network calls. Pull requests containing untyped remote calls or unverified client patterns fail CI checks and cannot be merged.
</enforcement>