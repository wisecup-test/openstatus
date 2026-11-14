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

These rules are ALWAYS ACTIVE for server-side HTTP route handlers, scheduled execution endpoints, and associated routing boundaries.

### Rules

- **R-NSM-001** MUST: All endpoints interfacing through next/server MUST return structured response objects compliant with server module interface specifications.
- **R-NSM-002** MANDATORY: Execute the Lock-Version Grounding procedure (discover manifest, build tool, lock artifact, official documentation, API existence check) before writing code that uses a versioned library.
- **R-NSM-003** MANDATORY: Isolate domain logic into dedicated internal coordinators and maintain next/server purely as an edge routing boundary.

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
Claude Code MUST NOT skip or defer verification. All server route handler endpoints are verified by automated continuous integration checks and peer code review; pull requests bypassing next/server or introducing unverified version dependencies are blocked from merging.
</enforcement>