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

These rules are ALWAYS ACTIVE for service definition modules, entity orchestration, persistence operations, and mutations spanning multiple relational entities, join tables, or cascading lifecycle states.

### Rules

- **R-DB-001** MUST: Before implementing or updating service boundary handlers that depend on @openstatus/db, developers MUST inspect the workspace dependency manifests and resolved lock artifacts to ground implementation against the exact version documentation.
- **R-DB-002** MUST: Sequence service operations to validate inputs and resolve authorization context prior to acquiring database transactions to minimize lock durations.
- **R-DB-003** MUST: Group cascading deletions and join table updates explicitly within the transaction block to preserve relational integrity across domain boundaries.

### Verify

```bash
# Discover the workspace test execution script from the repository manifest and run test suites covering service boundaries.
# Execute the project static analysis and type checking workflow to verify type compatibility across service handlers and database schema definitions.
```

**Accept when:**
- All service boundary operations managing multi-entity mutations execute within atomic transactions without orphaned join-table records.
- Verification suites and static analysis pass without type or transaction isolation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>