---
version: 2
name: cross-cutting-service-definitions-validate-parse-domai-6bae
description: "Encapsulating Service Transaction Boundaries with @openstatus/db: Service Definitions Validate Parse Domain Input"
metadata:
  actual:
    adr_id: 6bae1527-55e8-41b5-b2f7-1686a277db29
    status: accepted
    candidate_group_id: 1bb87db5-2722-4df2-bd84-8c3691b80aa2
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Encapsulating Service Transaction Boundaries with @openstatus/db: Service Definitions Validate Parse Domain Input

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** SHOULD: Service definitions validate and parse domain input payloads before opening database transactions or initiating queries against @openstatus/db schema entities.

### Verify

```bash
# Discover workspace test execution script from repository manifest and run test suites
# Execute project static analysis and type checking workflow
```

**Accept when:**
- All service boundary operations managing multi-entity mutations execute within atomic transactions without orphaned join-table records.
- Verification suites and static analysis pass without type or transaction isolation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated continuous integration checks and peer code review.
</enforcement>