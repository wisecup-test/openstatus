---
version: 2
name: cross-cutting-endpoints-interfacing-through-next-serve-02c9
description: "next/server Library Module Adoption: Endpoints Interfacing Through Next Server Return"
metadata:
  actual:
    adr_id: 02c9ad9e-83f1-404c-81eb-a92d07913a45
    status: accepted
    candidate_group_id: b6387b57-3b9c-4141-81e5-8184b16a040c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Library Module Adoption: Endpoints Interfacing Through Next Server Return

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for server-side HTTP route handlers, scheduled task execution routes, and all endpoints interfacing through next/server.

### Rules

- **R-NS-001** MUST: All endpoints interfacing through next/server MUST return structured response objects compliant with server module interface specifications.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>