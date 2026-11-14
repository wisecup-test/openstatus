---
version: 2
name: cross-cutting-status-page-proxy-resolution-subsystem-db3f
description: "Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Status Page Proxy Resolution Subsystem Consume"
metadata:
  actual:
    adr_id: db3fe22d-401c-4b1f-8fa2-cb835ac23bdc
    status: accepted
    candidate_group_id: f8b4ae84-5a62-426c-ba59-a50291856ca2
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/lib/proxy/**
---

# Adoption of @openstatus/db/src/schema for Proxy Resolution and Access Control: Status Page Proxy Resolution Subsystem Consume

Scope: applies to `apps/status-page/src/lib/proxy/**`.

These rules are ALWAYS ACTIVE for all status page proxy resolution handlers, access control predicate evaluators, and request rewrite logic matching the configured scope.

### Rules

- **R-PRX-001** MUST: The status page proxy resolution subsystem MUST consume @openstatus/db/src/schema as the single authoritative source for database entity structures and access policy models.
- **R-PRX-002** SHOULD: Import schema models as types whenever runtime database query builder objects are not required in proxy resolution.
- **R-PRX-003** SHOULD: Encapsulate schema evaluation logic inside dedicated predicate functions to maintain single-responsibility separation in routing handlers.

### Verify

```bash
# Discover and run the repository type-checking script to verify cross-package schema compatibility across proxy resolution handlers.
# Discover and execute the test runner for proxy routing resolution to validate access predicate behavior against schema models.
# Discover and execute the repository dependency boundary linter to confirm adherence to package import boundaries.
```

**Accept when:**
- Type checking passes without errors across all proxy resolution handlers importing @openstatus/db/src/schema.
- All proxy access control and rewrite unit tests pass successfully.
- No duplicate local interface definitions exist for entities provided by @openstatus/db/src/schema.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>