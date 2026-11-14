---
version: 2
name: cross-cutting-modules-executing-client-side-universal-f8fe
description: "Adoption of next/headers for Server-Side Request Context Management: Modules Executing Client Side Universal Execution"
metadata:
  actual:
    adr_id: f8fe9a1d-3b25-4be5-b2bb-18cfd1bb3728
    status: accepted
    candidate_group_id: 1c61ef2b-7759-42f4-82a3-4ce67ddac0d2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/lib/**
      - src/trpc/**
---

# Adoption of next/headers for Server-Side Request Context Management: Modules Executing Client Side Universal Execution

Scope: applies to `apps/web/src/lib/**`, `apps/web/src/trpc/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NXT-001** MUST_NOT: Modules executing in client-side or universal execution environments MUST NOT import or invoke functions from next/headers.

### Verify

```bash
# Discover and execute the repository static analysis and linting scripts to verify that next/headers imports are restricted to server-side modules.
# Discover and run the project test suite to confirm that server request context utilities correctly retrieve and sanitize headers and cookies under mock request environments.
```

**Accept when:**
- Static analysis confirms all imports of next/headers reside within designated server execution boundaries with zero client-side imports.
- Automated test executions verify that header forwarding routines strip hop-by-hop transport headers and cookie parsing validates input against schema definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification of static analysis rules and test suites is mandatory.
</enforcement>