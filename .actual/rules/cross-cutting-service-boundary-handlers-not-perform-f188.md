---
version: 2
name: cross-cutting-service-boundary-handlers-not-perform-f188
description: "Encapsulating Service Transaction Boundaries with @openstatus/db: Service Boundary Handlers Not Perform Cascading"
metadata:
  actual:
    adr_id: f1880aed-8ac5-44c8-a6df-3dbceedc5bea
    status: accepted
    candidate_group_id: 1bb87db5-2722-4df2-bd84-8c3691b80aa2
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Encapsulating Service Transaction Boundaries with @openstatus/db: Service Boundary Handlers Not Perform Cascading

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for service definition modules performing business logic, entity orchestration, or persistence operations, and mutations spanning multiple relational entities, join tables, or cascading lifecycle states.

### Rules

- **R-SVC-001** MUST_NOT: Service boundary handlers MUST NOT perform cascading deletions or join-table mutations outside of a shared transactional client context.

### Verify

```bash
# Discover the workspace test execution script from the repository manifest and run test suites covering service boundaries.
# Execute the project static analysis and type checking workflow to verify type compatibility across service handlers and database schema definitions.
```

**Accept when:**
- All service boundary operations managing multi-entity mutations execute within atomic transactions without orphaned join-table records.
- Verification suites and static analysis pass without type or transaction isolation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks execute service integration test suites and static type verification; peer code review verifies that multi-table state modifications are contained within transactional boundaries.
</enforcement>