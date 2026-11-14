---
version: 2
name: cross-cutting-engineers-verify-resolved-lock-file-4bc2
description: "@/lib/trpc/server Module Adoption for Server-Side Data Access: Engineers Verify Resolved Lock File Versions"
metadata:
  actual:
    adr_id: 4bc2f0b2-1fa8-43e2-a256-d33fe7a574e8
    status: accepted
    candidate_group_id: c87f948b-cbb0-46f6-8ca7-261dcab1560b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# @/lib/trpc/server Module Adoption for Server-Side Data Access: Engineers Verify Resolved Lock File Versions

Scope: applies to `apps/dashboard/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-TRPC-001** MUST: Engineers MUST verify the resolved lock-file versions for all underlying data access and procedure framework dependencies before implementing or modifying server caller bindings.

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