---
version: 2
name: cross-cutting-read-only-service-query-operations-d199
description: "Encapsulating Service Transaction Boundaries with @openstatus/db: Read Only Service Query Operations Execute"
metadata:
  actual:
    adr_id: d1992c93-f019-4c41-aed7-2320409d5dcc
    status: accepted
    candidate_group_id: 1bb87db5-2722-4df2-bd84-8c3691b80aa2
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Encapsulating Service Transaction Boundaries with @openstatus/db: Read Only Service Query Operations Execute

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for service definition modules performing business logic, entity orchestration, or persistence operations, and mutations spanning multiple relational entities, join tables, or cascading lifecycle states.

### Rules

- **R-DB-001** MAY: Read-only service query operations MAY execute against non-transactional database clients when atomic consistency across multiple tables is not required.

### Verify

```bash
# Discover workspace test execution script from repository manifest and run test suites
# Execute project static analysis and type checking workflow
```

**Accept when:**
- All service boundary operations managing multi-entity mutations execute within atomic transactions without orphaned join-table records.
- Verification suites and static analysis pass without type or transaction isolation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>