---
version: 2
name: cross-cutting-route-handler-implementations-not-bypass-423e
description: "next/server Library Module Adoption: Route Handler Implementations Not Bypass Next"
metadata:
  actual:
    adr_id: 423ef37a-0363-4c34-8911-8666831c41dc
    status: accepted
    candidate_group_id: b6387b57-3b9c-4141-81e5-8184b16a040c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Library Module Adoption: Route Handler Implementations Not Bypass Next

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for server-side HTTP route handler endpoints and scheduled task execution routes requiring structured HTTP dispatch and response handling.

### Rules

- **R-NS-001** MUST_NOT: Route handler implementations MUST NOT bypass next/server abstractions using non-standardized protocol layers or unmanaged runtime response channels.

### Verify

```bash
# Discover and run project static analysis, type checking, and automated test suites for server route handlers
# Inspect dependency resolution lock artifacts to verify server library module integrity
```

**Accept when:**
- All server-side route handlers compile cleanly and adhere to next/server module interfaces.
- Automated tests covering endpoint dispatch and response formats pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. All server route handlers must be verified via CI checks and code reviews against next/server standards.
</enforcement>