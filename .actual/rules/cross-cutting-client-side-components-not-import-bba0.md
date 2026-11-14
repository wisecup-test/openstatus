---
version: 2
name: cross-cutting-client-side-components-not-import-bba0
description: "@/lib/trpc/server Module Adoption for Server-Side Data Access: Client Side Components Not Import Directly"
metadata:
  actual:
    adr_id: bba0e3bb-f1fb-4a24-b92c-fed5f9d0605f
    status: accepted
    candidate_group_id: c87f948b-cbb0-46f6-8ca7-261dcab1560b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# @/lib/trpc/server Module Adoption for Server-Side Data Access: Client Side Components Not Import Directly

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for server-rendered layout and page components requiring internal server-side data fetching or procedure execution, and application route hierarchy components handling server context and initial data resolution.

### Rules

- **R-TRPC-001** MUST_NOT: Client-side components MUST NOT import or directly execute callers from @/lib/trpc/server.

### Verify

```bash
# Discover the repository package manifest and execute the workspace type verification script to ensure proper typing across all server callers.
# Discover the workspace linting configuration and run static analysis to verify that server caller imports are isolated from client component modules.
# Discover the repository test runner and execute the automated test suite covering server route rendering and procedure invocation.
```

**Accept when:**
- All server-side page and layout data fetches resolve successfully through @/lib/trpc/server without static type errors.
- No client-side components import or execute callers from @/lib/trpc/server.
- Continuous integration checks pass with zero static analysis or compilation violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>