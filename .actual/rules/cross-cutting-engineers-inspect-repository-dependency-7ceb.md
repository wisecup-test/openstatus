---
version: 2
name: cross-cutting-engineers-inspect-repository-dependency-7ceb
description: "Hono Routing and Context Propagation for HTTP Service Boundaries: Engineers Inspect Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 7ceb337a-4ef5-46dc-b7c4-bdd5b6156f9f
    status: accepted
    candidate_group_id: cd159887-e959-480c-aaac-a83ab1895daf
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/**
---

# Hono Routing and Context Propagation for HTTP Service Boundaries: Engineers Inspect Repository Dependency Manifest Lock

Scope: applies to `apps/server/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-HONO-001** MUST: Engineers MUST inspect the repository dependency manifest and lock artifact to identify the exact resolved version of Hono before authoring or modifying service boundary endpoints.

### Verify

```bash
# Discover and execute the test runner command from the repository configuration to validate HTTP route definitions and status codes.
# Discover and execute the static analysis/type checking command from the workspace manifest to ensure Hono Context variables adhere to expected type declarations.
```

**Accept when:**
- All HTTP endpoints successfully route requests and respond with expected status codes and payloads through the framework pipeline.
- Static type analysis passes with zero type errors for context setter and getter invocations across route handlers and middleware.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>