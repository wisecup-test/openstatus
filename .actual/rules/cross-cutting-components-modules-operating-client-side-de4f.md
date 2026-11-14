---
version: 2
name: cross-cutting-components-modules-operating-client-side-de4f
description: "Server Header and Cookie Management with next/headers: Components Modules Operating Client Side Not"
metadata:
  actual:
    adr_id: de4ff98d-0fb6-412a-ba4f-32b3aac50a81
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

# Server Header and Cookie Management with next/headers: Components Modules Operating Client Side Not

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for server components, server layouts, server actions, API route handlers, and server utility modules requiring HTTP cookie access or request header inspection.

### Rules

- **R-NXT-001** MUST_NOT: Components and modules operating on the client side MUST NOT import or invoke functions from the next/headers module.

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
Claude Code MUST NOT skip or defer verification. All modifications involving server headers and cookies must pass static analysis, code review, and automated test suites.
</enforcement>