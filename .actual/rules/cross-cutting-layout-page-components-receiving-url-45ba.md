---
version: 2
name: cross-cutting-layout-page-components-receiving-url-45ba
description: "@/lib/trpc/server Module Adoption for Server-Side Data Access: Layout Page Components Receiving Url Search"
metadata:
  actual:
    adr_id: 45ba6bfa-544b-4350-aced-be48b490de5d
    status: accepted
    candidate_group_id: c87f948b-cbb0-46f6-8ca7-261dcab1560b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# @/lib/trpc/server Module Adoption for Server-Side Data Access: Layout Page Components Receiving Url Search

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all server-rendered layout and page components requiring internal server-side data fetching or procedure execution.

### Rules

- **R-TRPC-001** SHOULD: Layout and page components receiving URL search parameters SHOULD coordinate input validation prior to downstream procedure dispatch via @/lib/trpc/server.

### Verify

```bash
# Discover the repository package manifest and execute workspace type verification
npm run typecheck

# Discover the workspace linting configuration and run static analysis
npm run lint

# Discover the repository test runner and execute the automated test suite
npm test
```

**Accept when:**
- All server-side page and layout data fetches resolve successfully through @/lib/trpc/server without static type errors.
- No client-side components import or execute callers from @/lib/trpc/server.
- Continuous integration checks pass with zero static analysis or compilation violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>