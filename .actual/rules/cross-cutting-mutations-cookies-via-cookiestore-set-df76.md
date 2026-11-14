---
version: 2
name: cross-cutting-mutations-cookies-via-cookiestore-set-df76
description: "Server Header and Cookie Management with next/headers: Mutations Cookies Via Cookiestore Set Operations"
metadata:
  actual:
    adr_id: df768e53-b8a0-4afe-b43b-daeb8ac45257
    status: accepted
    candidate_group_id: eeb8ffd4-578b-4c7f-bfab-122400d7811e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/lib/**
---

# Server Header and Cookie Management with next/headers: Mutations Cookies Via Cookiestore Set Operations

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-COOKIE-001** MUST_NOT: Mutations to cookies via cookieStore set operations MUST NOT be performed inside read-only server component renders or layouts.

### Verify

```bash
# Discover the project manifest and execute type-checking and compilation commands
# Discover and run the project code quality and linting verification scripts
# Discover and run the automated test suite
```

**Accept when:**
- Type-checking and compilation tasks complete with zero errors related to next/headers module resolution or invocation signatures.
- Code quality checks complete with zero warnings or errors regarding forbidden client-side imports or invalid server-side cookie mutation.
- Automated functional and integration tests covering server cookie management and header extraction pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>