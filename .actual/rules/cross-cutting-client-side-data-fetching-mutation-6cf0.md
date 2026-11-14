---
version: 2
name: cross-cutting-client-side-data-fetching-mutation-6cf0
description: "Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Client Side Data Fetching Mutation Invocations"
metadata:
  actual:
    adr_id: 6cf0e5fd-73ce-48bd-93f7-e0969367b62e
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

# Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Client Side Data Fetching Mutation Invocations

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all client components, interactive views, forms, and server-side client proxy utilities invoking remote procedures.

### Rules

- **R-TRPC-001** SHOULD: Client-side data fetching and mutation invocations coordinate @trpc/client through paired query and mutation hooks rather than invoking raw client procedure promises directly in UI lifecycle methods.
- **R-TRPC-002** MANDATORY: The consumer MUST discover tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-TRPC-003** MANDATORY: Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact version, (4) Look up official docs for that exact version, (5) Confirm every API/class/function exists in that version's docs, (6) Re-run steps 3-5 per dependency at point of use.
- **R-TRPC-004** MANDATORY: Configure shared client links to attach standard telemetry logging, request headers, and custom serialization transformers across all client instances.
- **R-TRPC-005** MANDATORY: Ensure server-executed client instances forward incoming session cookies and authentication headers while enforcing server execution boundaries.

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
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines run automated type checking and test suites on pull requests, and peer code review verifies compliance with client architecture and the absence of untyped network calls.
</enforcement>