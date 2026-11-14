---
version: 2
name: cross-cutting-server-side-http-route-handlers-aa09
description: "next/server Library Module Adoption: Server Side Http Route Handlers Scheduled"
metadata:
  actual:
    adr_id: aa099e92-9285-4385-9604-90dbec95a3a3
    status: accepted
    candidate_group_id: b6387b57-3b9c-4141-81e5-8184b16a040c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Library Module Adoption: Server Side Http Route Handlers Scheduled

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for server-side HTTP route handlers and scheduled invocation endpoints.

### Rules

- **R-SRV-001** MUST: Server-side HTTP route handlers and scheduled invocation endpoints MUST use next/server as the authoritative server module for processing incoming requests and constructing responses.

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
Claude Code MUST NOT skip or defer verification. All server-side route handlers must adhere to next/server module interfaces and pass automated tests.
</enforcement>