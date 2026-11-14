---
version: 2
name: cross-cutting-scheduled-task-endpoints-next-server-31c5
description: "next/server Library Module Adoption: Scheduled Task Endpoints Next Server Delegate"
metadata:
  actual:
    adr_id: 31c51fb2-7a19-4dad-b1c3-ea0747861d55
    status: accepted
    candidate_group_id: b6387b57-3b9c-4141-81e5-8184b16a040c
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/app/api/checker/cron/**
---

# next/server Library Module Adoption: Scheduled Task Endpoints Next Server Delegate

Scope: applies to `apps/web/src/app/api/checker/cron/**`.

These rules are ALWAYS ACTIVE for all server-side HTTP route handler endpoints and scheduled task execution routes.

### Rules

- **R-NXT-001** SHOULD: Scheduled task endpoints using next/server delegate execution logic to dedicated internal domain coordinators and report execution state to the standardized internal telemetry module.
- **R-NXT-002** MANDATORY: Execute the LOCK-VERSION GROUNDING procedure (find manifest, identify build tool, inspect lock artifact for resolved version, look up official version-specific documentation, confirm API existence) before writing code that uses a versioned library.
- **R-NXT-003** MANDATORY: Keep route handler functions focused on request validation, delegating actual task execution to internal modules.
- **R-NXT-004** MANDATORY: Ensure response payloads adhere to consistent structured representations for automated monitoring.

### Verify

```bash
# Discover and run the project static analysis and type checking verification scripts
# Discover and execute automated test suites covering server route handlers
# Inspect dependency resolution lock artifacts to verify server library module integrity
```

**Accept when:**
- All server-side route handlers compile cleanly and adhere to next/server module interfaces.
- Automated tests covering endpoint dispatch and response formats pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. All server route handler endpoints must adhere to next/server module interfaces and pass automated tests.
</enforcement>