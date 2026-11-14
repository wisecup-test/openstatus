---
version: 2
name: cross-cutting-proxy-handlers-evaluating-access-restric-d7f9
description: "Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Proxy Handlers Evaluating Access Restrictions Domain"
metadata:
  actual:
    adr_id: d7f9ad62-e67c-4c40-97f8-8a017b541c63
    status: accepted
    candidate_group_id: f8b4ae84-5a62-426c-ba59-a50291856ca2
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Proxy Handlers Evaluating Access Restrictions Domain

Scope: applies to `apps/status-page/src/lib/proxy/**`.

These rules are ALWAYS ACTIVE for status page proxy resolution handlers, access control predicate evaluators, and request rewrite logic.

### Rules

- **R-PROXY-001** SHOULD: Proxy handlers evaluating access restrictions or domain rewrites SHOULD isolate schema field evaluations within modular access predicates and path builders.
- **R-PROXY-002** MANDATORY: Import schema models as types whenever runtime database query builder objects are not required in proxy resolution.
- **R-PROXY-003** MANDATORY: Encapsulate schema evaluation logic inside dedicated predicate functions to maintain single-responsibility separation in routing handlers.

### Verify

```bash
# Discover and run the repository type-checking script to verify cross-package schema compatibility across proxy resolution handlers
# Discover and execute the test runner for proxy routing resolution to validate access predicate behavior against schema models
# Discover and execute the repository dependency boundary linter to confirm adherence to package import boundaries
```

**Accept when:**
- Type checking passes without errors across all proxy resolution handlers importing @openstatus/db/src/schema.
- All proxy access control and rewrite unit tests pass successfully.
- No duplicate local interface definitions exist for entities provided by @openstatus/db/src/schema.

<enforcement>
Claude Code MUST NOT skip or defer verification. All changes must pass CI type checking, unit tests, and dependency boundary linters without violations.
</enforcement>