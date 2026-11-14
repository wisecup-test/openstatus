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

These rules are ALWAYS ACTIVE for server-side HTTP route handler endpoints and scheduled task execution routes requiring structured HTTP dispatch and response handling.

### Rules

- **R-NSL-001** MUST: Before implementing or updating endpoints utilizing next/server, the consumer MUST inspect the repository dependency resolution lock artifact to determine the exact resolved version and verify that all invoked server module interfaces exist in that version.
- **R-NSL-002** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-NSL-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find the dependency manifest in the repo; (2) Identify the build tool; (3) Inspect the repository lock or resolution artifact for the exact resolved version; (4) Look up official documentation/changelog/public API reference for that exact version; (5) Confirm every API, class, or function exists in that version's documentation; (6) Re-run steps 3-5 per dependency at point of use for version-sensitive behavior.
- **R-NSL-004** SHOULD: Keep route handler functions focused on request validation, delegating actual task execution to internal modules.
- **R-NSL-005** SHOULD: Ensure response payloads adhere to consistent structured representations for automated monitoring.

### Verify

```bash
# Discover and run project static analysis and type checking verification scripts
# Discover and execute automated test suites covering server route handlers
# Inspect dependency resolution lock artifacts to verify server library module integrity
```

**Accept when:**
- All server-side route handlers compile cleanly and adhere to next/server module interfaces.
- Automated tests covering endpoint dispatch and response formats pass without regression.

<enforcement>
Claude Code MUST NOT skip or defer verification. All new and updated server route handler endpoints are verified by automated continuous integration checks and peer code reviews; pull requests bypassing next/server or introducing unverified version dependencies are blocked from merging.
</enforcement>