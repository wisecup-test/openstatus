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
- **R-TRPC-002** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-TRPC-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact for exact version, (4) Look up official documentation for that exact version, (5) Confirm every API/class/function exists in that version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-TRPC-004** MUST: Configure shared client links to attach standard telemetry logging, request headers, and custom serialization transformers across all client instances.
- **R-TRPC-005** MUST: Ensure server-executed client instances forward incoming session cookies and authentication headers while enforcing server execution boundaries.

### Verify

```bash
# Discover workspace verification script in repository manifest and execute type checking across client and API packages
# Locate repository testing script from configuration files and execute test suite
# Inspect workspace linting scripts and execute static analysis to verify no direct untyped network client calls bypass the remote procedure layer
```

**Accept when:**
- Static type analysis completes with zero errors across all client remote procedure invocation points.
- All automated integration and component tests exercising client queries and mutations execute successfully.
- Code review confirms all new client data fetching and mutation operations route through the standardized remote procedure client.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines run automated type checking and test suites on pull requests, and peer code review verifies compliance with client architecture and absence of untyped network calls. Pull requests containing untyped remote calls or unverified client patterns fail continuous integration checks and cannot be merged.
</enforcement>