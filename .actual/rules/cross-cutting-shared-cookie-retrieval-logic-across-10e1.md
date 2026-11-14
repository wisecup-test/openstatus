---
version: 2
name: cross-cutting-shared-cookie-retrieval-logic-across-10e1
description: "Server Header and Cookie Management with next/headers: Shared Cookie Retrieval Logic Across Layouts"
metadata:
  actual:
    adr_id: 10e167f2-8b87-4553-8913-bba6b7f55502
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

# Server Header and Cookie Management with next/headers: Shared Cookie Retrieval Logic Across Layouts

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for server components, server layouts, server actions, API route handlers, and server utility modules requiring HTTP cookie access or request header inspection.

### Rules

- **R-COOKIE-001** SHOULD: Shared cookie retrieval logic across layouts and pages SHOULD be encapsulated within dedicated server-side utility functions rather than duplicating direct cookieStore access across multiple callers.
- **R-COOKIE-002** MANDATORY: When retrieving cookie values inside server layouts or pages, use helper functions to provide fallback defaults when the target cookie is unset.
- **R-COOKIE-003** MANDATORY: Ensure that all cookie write operations specify appropriate path, maxAge, sameSite, httpOnly, and environment-dependent secure attributes.

### Verify

```bash
# Discover the project manifest and execute the designated type-checking and compilation commands
# Discover and run the project code quality and linting verification scripts
# Discover and run the automated test suite
```

**Accept when:**
- Type-checking and compilation tasks complete with zero errors related to next/headers module resolution or invocation signatures.
- Code quality checks complete with zero warnings or errors regarding forbidden client-side imports or invalid server-side cookie mutation.
- Automated functional and integration tests covering server cookie management and header extraction pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis checks and peer code reviews enforce compliance.
</enforcement>