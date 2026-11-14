---
version: 2
name: cross-cutting-client-instances-created-via-trpc-adcf
description: "Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Client Instances Created Via Trpc Configure"
metadata:
  actual:
    adr_id: adcff326-5681-4a1f-9972-d7756d79839d
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

# Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Client Instances Created Via Trpc Configure

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/components/**`, `apps/dashboard/src/lib/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Client instances created via @trpc/client MUST configure standardized serialization and error handling links to maintain consistent payload transformations and error logging.

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