---
version: 2
name: cross-cutting-before-implementing-updating-endpoints-u-84ab
description: "next/server Library Module Adoption: Before Implementing Updating Endpoints Utilizing Next"
metadata:
  actual:
    adr_id: 84ab19ac-0a48-4a7c-8719-11466cccfed6
    status: accepted
    candidate_group_id: b6387b57-3b9c-4141-81e5-8184b16a040c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Library Module Adoption: Before Implementing Updating Endpoints Utilizing Next

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for all server-side HTTP route handlers, scheduled task execution routes, and related server-side endpoint updates utilizing next/server.

### Rules

- **R-NEXT-001** MUST: Before implementing or updating endpoints utilizing next/server, the consumer MUST inspect the repository dependency resolution lock artifact to determine the exact resolved version and verify that all invoked server module interfaces exist in that version.

### Verify

```bash
# Discover and run the project static analysis and type checking verification scripts.
# Discover and execute automated test suites covering server route handlers.
# Inspect dependency resolution lock artifacts to verify server library module integrity.
```

**Accept when:**
- All server-side route handlers compile cleanly and adhere to next/server module interfaces.
- Automated tests covering endpoint dispatch and response formats pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new and updated server route handler endpoints must adhere to next/server module interfaces and verify version compatibility against dependency resolution lock artifacts.
</enforcement>