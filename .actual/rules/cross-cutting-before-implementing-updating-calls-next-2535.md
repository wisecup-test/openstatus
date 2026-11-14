---
version: 2
name: cross-cutting-before-implementing-updating-calls-next-2535
description: "Server Header and Cookie Management with next/headers: Before Implementing Updating Calls Next Headers"
metadata:
  actual:
    adr_id: 25351221-fce4-4501-9149-3de9fce0dc9b
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

# Server Header and Cookie Management with next/headers: Before Implementing Updating Calls Next Headers

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for server components, server layouts, server actions, API route handlers, and server utility modules requiring HTTP cookie access or request header inspection.

### Rules

- **R-NXT-001** MUST: Before implementing or updating calls to the next/headers module, the consumer MUST inspect the repository lock artifact to determine the exact locked version of the framework and verify API compatibility against the official documentation for that resolved version.

### Verify

```bash
# Discover the project manifest and execute the designated type-checking and compilation commands
npm run typecheck || npx tsc --noEmit
# Run code quality and linting verification scripts
npm run lint
# Run the automated test suite
npm test
```

**Accept when:**
- Type-checking and compilation tasks complete with zero errors related to next/headers module resolution or invocation signatures.
- Code quality checks complete with zero warnings or errors regarding forbidden client-side imports or invalid server-side cookie mutation.
- Automated functional and integration tests covering server cookie management and header extraction pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All modifications involving server-side header and cookie management with next/headers must be statically analyzed, peer-reviewed, and verified against lockfile-grounded framework versions.
</enforcement>