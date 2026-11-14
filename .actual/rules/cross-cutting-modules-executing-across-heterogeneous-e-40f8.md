---
version: 2
name: cross-cutting-modules-executing-across-heterogeneous-e-40f8
description: "Next.js Runtime Environment Variable Configuration: Modules Executing Across Heterogeneous Execution Targets"
metadata:
  actual:
    adr_id: 40f8c9dd-ecf1-4d79-8ab2-bb183d947f90
    status: accepted
    candidate_group_id: 01a7d574-6a6a-490d-a13f-d7c47d691d1a
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - instrumentation-client.ts
      - next.config.ts
      - src/**
---

# Next.js Runtime Environment Variable Configuration: Modules Executing Across Heterogeneous Execution Targets

Scope: applies to `apps/web/instrumentation-client.ts`, `apps/web/next.config.ts`, `apps/web/src/**`.

These rules are ALWAYS ACTIVE for server-side route handlers, background cron jobs, server-rendered components, client instrumentation hooks, and shared protocol utilities participating in runtime code execution.

### Rules

- **R-ENV-001** SHOULD: Modules executing across heterogeneous execution targets SHOULD inspect the framework runtime environment property to branch between edge and standard runtime initialization logic.
- **R-ENV-002** MANDATORY: Consumers MUST derive all tool names, file names, commands, package managers, and version numbers directly from the project repository.
- **R-ENV-003** MANDATORY: Before writing code that uses a versioned library, execute the lock-version grounding process: find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for the exact resolved version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run per dependency at point of use for version-sensitive behavior.
- **R-ENV-004** MANDATORY: Ensure that all environment variable reads include appropriate fallback values or explicit validation when invoked inside critical execution paths.
- **R-ENV-005** MANDATORY: Maintain strict boundary isolation to guarantee that private authorization keys and webhook targets remain confined to server execution contexts.

### Verify

```bash
echo "Discover project verification scripts from repository manifest and execute environment check commands"
echo "Inspect codebase for direct process environment variable references across module boundaries"
```

**Accept when:**
- All server route handlers, background tasks, and shared client-server modules resolve required runtime configuration properties without unhandled runtime exceptions
- Client-side bundles remain completely free of server-only secrets and private credential references

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>