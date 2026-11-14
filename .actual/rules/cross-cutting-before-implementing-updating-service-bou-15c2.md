---
version: 2
name: cross-cutting-before-implementing-updating-service-bou-15c2
description: "Encapsulating Service Transaction Boundaries with @openstatus/db: Before Implementing Updating Service Boundary Handlers"
metadata:
  actual:
    adr_id: 15c21b46-5852-43b2-ae62-5ffbd11680c0
    status: accepted
    candidate_group_id: 1bb87db5-2722-4df2-bd84-8c3691b80aa2
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Encapsulating Service Transaction Boundaries with @openstatus/db: Before Implementing Updating Service Boundary Handlers

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for service definition modules performing business logic, entity orchestration, or persistence operations involving multi-entity mutations.

### Rules

- **R-DB-001** MUST: Before implementing or updating service boundary handlers that depend on @openstatus/db, developers MUST inspect the workspace dependency manifests and resolved lock artifacts to ground implementation against the exact version documentation.

### Verify

```bash
# Discover and execute the workspace test execution script and static analysis/type checking workflow from the repository manifest
# Example discovery pattern:
# npm test || pnpm test || yarn test
# npm run typecheck || pnpm typecheck || yarn typecheck
```

**Accept when:**
- All service boundary operations managing multi-entity mutations execute within atomic transactions without orphaned join-table records.
- Verification suites and static analysis pass without type or transaction isolation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks, peer code reviews, and static type verification enforce these rules, and non-compliant pull requests are blocked from merging.
</enforcement>