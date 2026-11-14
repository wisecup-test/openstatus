---
version: 2
name: cross-cutting-new-server-side-endpoints-requiring-93dc
description: "Internal tRPC Server Module Adoption for Server-Side Procedure Invocation: New Server Side Endpoints Requiring Authenticated"
metadata:
  actual:
    adr_id: 93dc375c-10ae-4712-b5a0-9232a3a26ea9
    status: accepted
    candidate_group_id: 7fe2d577-a00a-412e-b5f2-7899a8fc560f
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Internal tRPC Server Module Adoption for Server-Side Procedure Invocation: New Server Side Endpoints Requiring Authenticated

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, layout components, and server components executing in the application server runtime requiring authenticated or cached domain data.

### Rules

- **R-TRPC-001** SHOULD: New server-side endpoints requiring authenticated or cached domain data SHOULD consume the standardized caller exported by the internal tRPC server module.
- **R-TRPC-002** MANDATORY: Derive the server caller initialization pattern by referencing the project internal tRPC server configuration module.
- **R-TRPC-003** MANDATORY: Ensure request headers and authentication cookies are forwarded into the procedure context when instantiating the server caller in route handlers.

### Verify

```bash
# Discover and execute project verification suites through configured repository task runner
# Run repository static analysis to verify all server-side procedure invocations use the internal tRPC server module
```

**Accept when:**
- All server-side route handlers and layouts invoke backend operations exclusively via the internal tRPC server caller.
- Repository static analysis and type verification suites pass without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>