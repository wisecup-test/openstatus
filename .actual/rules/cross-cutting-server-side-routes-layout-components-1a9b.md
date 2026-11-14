---
version: 2
name: cross-cutting-server-side-routes-layout-components-1a9b
description: "Internal tRPC Server Module Adoption for Server-Side Procedure Invocation: Server Side Routes Layout Components Requiring"
metadata:
  actual:
    adr_id: 1a9baf7e-ced6-4369-8293-2062d5b6717d
    status: accepted
    candidate_group_id: 7fe2d577-a00a-412e-b5f2-7899a8fc560f
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Internal tRPC Server Module Adoption for Server-Side Procedure Invocation: Server Side Routes Layout Components Requiring

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Server-side routes and layout components requiring backend procedure execution MUST route their operations through the internal tRPC server module.

### Verify

```bash
sh -c 'echo "Discover and execute project verification suites through configured repository task runner"'
sh -c 'echo "Run repository static analysis to verify all server-side procedure invocations use the internal tRPC server module"'
```

**Accept when:**
- All server-side route handlers and layouts invoke backend operations exclusively via the internal tRPC server caller.
- Repository static analysis and type verification suites pass without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>