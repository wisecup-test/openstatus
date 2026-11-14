---
version: 2
name: cross-cutting-dependent-join-table-associations-auxili-223c
description: "Encapsulating Service Transaction Boundaries with @openstatus/db: Dependent Join Table Associations Auxiliary Entity"
metadata:
  actual:
    adr_id: 223cbf8d-8e00-4b8b-8f8e-5b7a97d16e73
    status: accepted
    candidate_group_id: 1bb87db5-2722-4df2-bd84-8c3691b80aa2
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Encapsulating Service Transaction Boundaries with @openstatus/db: Dependent Join Table Associations Auxiliary Entity

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for service definition modules performing business logic, entity orchestration, or persistence operations across multiple relational entities, join tables, or cascading lifecycle states.

### Rules

- **R-DB-001** MUST: Dependent join table associations and auxiliary entity records MUST be deleted or reconciled within the same transaction that modifies or removes the parent entity.

### Verify

```bash
# Discover the workspace test execution script from the repository manifest and run test suites covering service boundaries.
# Execute the project static analysis and type checking workflow to verify type compatibility across service handlers and database schema definitions.
```

**Accept when:**
- All service boundary operations managing multi-entity mutations execute within atomic transactions without orphaned join-table records.
- Verification suites and static analysis pass without type or transaction isolation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. All multi-table state modifications must be contained within transactional boundaries, verified via continuous integration checks and peer code review.
</enforcement>