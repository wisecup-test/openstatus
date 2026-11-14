---
version: 2
name: cross-cutting-server-side-route-handlers-layout-ec1e
description: "Internal tRPC Server Module Adoption for Server-Side Procedure Invocation: Server Side Route Handlers Layout Components"
metadata:
  actual:
    adr_id: ec1e1819-754d-4ec2-a6cd-2fdcb5492eec
    status: accepted
    candidate_group_id: 7fe2d577-a00a-412e-b5f2-7899a8fc560f
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Internal tRPC Server Module Adoption for Server-Side Procedure Invocation: Server Side Route Handlers Layout Components

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, layout components, and server components executing in the application server runtime requiring direct domain data retrieval.

### Rules

- **R-TRPC-001** MUST_NOT: Server-side route handlers and layout components MUST NOT invoke internal procedures by issuing HTTP fetch requests to their own internal endpoints.

### Verify

```bash
# Discover and execute project verification suites through configured repository task runner
# Run repository static analysis to verify all server-side procedure invocations use the internal tRPC server module
```

**Accept when:**
- All server-side route handlers and layouts invoke backend operations exclusively via the internal tRPC server caller.
- Repository static analysis and type verification suites pass without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis pipelines and mandatory peer code reviews enforce compliance.
</enforcement>