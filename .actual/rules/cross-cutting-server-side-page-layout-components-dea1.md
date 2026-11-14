---
version: 2
name: cross-cutting-server-side-page-layout-components-dea1
description: "@/lib/trpc/server Module Adoption for Server-Side Data Access: Server Side Page Layout Components Requiring"
metadata:
  actual:
    adr_id: dea19350-5a4c-4e2f-a09c-c382b32e0205
    status: accepted
    candidate_group_id: c87f948b-cbb0-46f6-8ca7-261dcab1560b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# @/lib/trpc/server Module Adoption for Server-Side Data Access: Server Side Page Layout Components Requiring

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Server-side page and layout components requiring backend procedure calls or data prefetching MUST import and invoke server procedures via the @/lib/trpc/server module rather than issuing external HTTP requests or instantiating duplicate database clients directly.

### Verify

```bash
# Discover the workspace package manager and run type verification
PKG_MANAGER=$(if [ -f pnpm-lock.yaml ]; then echo "pnpm"; elif [ -f yarn.lock ]; then echo "yarn"; else echo "npm"; fi)
$PKG_MANAGER run typecheck || $PKG_MANAGER run check || npx tsc --noEmit

# Run linting to verify module boundaries and isolation
$PKG_MANAGER run lint || npx eslint .

# Run automated tests covering server route rendering and procedure invocation
$PKG_MANAGER test
```

**Accept when:**
- All server-side page and layout data fetches resolve successfully through @/lib/trpc/server without static type errors.
- No client-side components import or execute callers from @/lib/trpc/server.
- Continuous integration checks pass with zero static analysis or compilation violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>