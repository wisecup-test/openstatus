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
- **R-TRPC-002** MANDATORY: The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-TRPC-003** MANDATORY: Prior to writing code that uses a versioned library, execute the lock-version grounding sequence (manifest discovery, build tool identification, lock file inspection for exact resolved version, documentation lookup, API existence confirmation, and re-running per dependency at point of use).

### Verify

```bash
sh -c 'echo "Discover and execute project verification suites through configured repository task runner"'
sh -c 'echo "Run repository static analysis to verify all server-side procedure invocations use the internal tRPC server module"'
```

**Accept when:**
- All server-side route handlers and layouts invoke backend operations exclusively via the internal tRPC server caller.
- Repository static analysis and type verification suites pass without errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis pipelines and mandatory peer code review enforce these rules, blocking pull requests that bypass the internal server caller.
</enforcement>