---
version: 2
name: cross-cutting-before-implementing-updating-any-depende-960a
description: "Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Before Implementing Updating Any Dependency Nuqs"
metadata:
  actual:
    adr_id: 960a06f1-795a-49d9-8a3a-be031911d4e1
    status: accepted
    candidate_group_id: b6caa4e1-a734-4f95-91e7-b9071cb6b4c1
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Before Implementing Updating Any Dependency Nuqs

Scope: applies to `apps/status-page/src/app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NUQS-001** MUST: Before implementing or updating any dependency on nuqs or nuqs/server, the consumer MUST inspect the repository lock artifact to determine the exact resolved library version and verify API compatibility against that specific version reference.

### Verify

```bash
# Discover and execute the repository static type checker
# (Command discovered from repository configuration)
npm run typecheck || pnpm typecheck || yarn typecheck

# Discover and run the project test suite
npm test || pnpm test || yarn test
```

**Accept when:**
- Static type checking and automated test suites confirm all route search parameters pass through validated cache parser instances.
- No server route handlers access raw search query parameters directly without parsing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>