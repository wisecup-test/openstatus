---
version: 2
name: cross-cutting-consumer-discover-ecosystem-lock-file-d9f8
description: "@libsql/client Primary Datastore Client Instantiation: Consumer Discover Ecosystem Lock File Verify"
metadata:
  actual:
    adr_id: d9f8c484-b710-45c1-b439-1f383a4de3f0
    status: accepted
    candidate_group_id: 57db3caa-0564-41c5-b9bd-ce20d7cb43f6
    evidence:
      source: deep
  scope:
    root: packages/tinybird
    paths:
      - scripts/**
---

# @libsql/client Primary Datastore Client Instantiation: Consumer Discover Ecosystem Lock File Verify

Scope: applies to `packages/tinybird/scripts/**`.

These rules are ALWAYS ACTIVE for standalone scripts, operational migration utilities, and background jobs connecting directly to the primary relational datastore.

### Rules

- **R-LIB-001** MUST: The consumer MUST discover the ecosystem lock file and verify the exact resolved version of `@libsql/client` against official documentation before implementation.

### Verify

```bash
# Discover and run project static analysis, type checking, and test suites
npm run lint || pnpm lint || yarn lint
npm run typecheck || pnpm typecheck || yarn typecheck
npm test || pnpm test || yarn test
```

**Accept when:**
- Static analysis and type checking verification scripts complete with zero errors.
- Primary datastore client connections instantiate successfully using validated configuration parameters.
- All automated test suites covering datastore operations pass without failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification by automated continuous integration checks, peer code review, and linting is mandatory.
</enforcement>