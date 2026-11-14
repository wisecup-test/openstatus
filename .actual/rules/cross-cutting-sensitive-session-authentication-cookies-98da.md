---
version: 2
name: cross-cutting-sensitive-session-authentication-cookies-98da
description: "Server Header and Cookie Management with next/headers: Sensitive Session Authentication Cookies Written Through"
metadata:
  actual:
    adr_id: 98daecbf-69d2-44fc-bcfa-123fab3cffa3
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

# Server Header and Cookie Management with next/headers: Sensitive Session Authentication Cookies Written Through

Scope: applies to `apps/dashboard/src/app/**`, `apps/dashboard/src/lib/**`.

These rules are ALWAYS ACTIVE for server components, server layouts, server actions, API route handlers, and server utility modules requiring HTTP cookie access or request header inspection.

### Rules

- **R-SHC-001** MUST: Sensitive session and authentication cookies written through cookieStore set operations MUST configure explicit security attributes ensuring HTTP-only access, secure transport in non-development environments, strict or lax same-site policy, and a bounded maximum age.

### Verify

```bash
# Discover the project manifest and execute type-checking and compilation commands
npm run build || pnpm build || yarn build || bun run build

# Discover and run code quality and linting verification scripts
npm run lint || pnpm lint || yarn lint || bun run lint

# Discover and run the automated test suite
npm test || pnpm test || yarn test || bun test
```

**Accept when:**
- Type-checking and compilation tasks complete with zero errors related to next/headers module resolution or invocation signatures.
- Code quality checks complete with zero warnings or errors regarding forbidden client-side imports or invalid server-side cookie mutation.
- Automated functional and integration tests covering server cookie management and header extraction pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests violating cookie security attribute requirements or attempting cookie writes in read-only render contexts are blocked from merging until corrected.
</enforcement>