---
version: 2
name: cross-cutting-server-components-execute-data-fetching-16a0
description: "@/lib/trpc/server Module Adoption for Server-Side Data Access: Server Components Execute Data Fetching Operations"
metadata:
  actual:
    adr_id: 16a0ea69-d699-49c6-907a-acdaa3ab99f5
    status: accepted
    candidate_group_id: c87f948b-cbb0-46f6-8ca7-261dcab1560b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# @/lib/trpc/server Module Adoption for Server-Side Data Access: Server Components Execute Data Fetching Operations

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for server-rendered layout and page components requiring internal server-side data fetching or procedure execution.

### Rules

- **R-TRPC-001** MUST: Server components MUST execute data fetching operations asynchronously using @/lib/trpc/server callers within server boundary contexts to maintain direct internal execution paths.

### Verify

```bash
# Discover workspace package manifest and run type verification script
# Example discovery-based command structure:
pnpm typecheck

# Run static analysis / linting to verify server caller imports are isolated
pnpm lint

# Run automated test suite covering server route rendering and procedure invocation
pnpm test
```

**Accept when:**
- All server-side page and layout data fetches resolve successfully through @/lib/trpc/server without static type errors.
- No client-side components import or execute callers from @/lib/trpc/server.
- Continuous integration checks pass with zero static analysis or compilation violations.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>