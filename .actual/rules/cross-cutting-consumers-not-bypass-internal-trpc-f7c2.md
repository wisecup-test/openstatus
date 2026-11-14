---
version: 2
name: cross-cutting-consumers-not-bypass-internal-trpc-f7c2
description: "Internal tRPC Server Module Adoption for Server-Side Procedure Invocation: Consumers Not Bypass Internal Trpc Server"
metadata:
  actual:
    adr_id: f7c21563-81e4-45e2-809f-884f4d71bbe1
    status: accepted
    candidate_group_id: 7fe2d577-a00a-412e-b5f2-7899a8fc560f
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Internal tRPC Server Module Adoption for Server-Side Procedure Invocation: Consumers Not Bypass Internal Trpc Server

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST_NOT: Consumers MUST NOT bypass the internal tRPC server module to execute direct queries for domain entities that are already encapsulated by established procedures.

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