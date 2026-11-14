---
version: 2
name: cross-cutting-server-side-components-actions-route-fc14
description: "Server Header and Cookie Management with next/headers: Server Side Components Actions Route Handlers"
metadata:
  actual:
    adr_id: fc1449a1-8494-4faa-aa8c-2fb7c1d54e53
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

# Server Header and Cookie Management with next/headers: Server Side Components Actions Route Handlers

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for server components, server layouts, server actions, API route handlers, and server utility modules requiring HTTP cookie access or request header inspection.

### Rules

- **R-HDR-001** MUST: Server-side components, server actions, route handlers, and server-scoped utility modules MUST manage incoming HTTP headers and HTTP cookies exclusively through the next/headers module.

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
Claude Code MUST NOT skip or defer verification. All modifications involving authentication, session cookies, and route handlers must pass static analysis and peer code review.
</enforcement>