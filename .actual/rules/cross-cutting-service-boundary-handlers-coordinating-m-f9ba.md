---
version: 2
name: cross-cutting-service-boundary-handlers-coordinating-m-f9ba
description: "Encapsulating Service Transaction Boundaries with @openstatus/db: Service Boundary Handlers Coordinating Multi Entity"
metadata:
  actual:
    adr_id: f9bac8d3-44f1-4991-972d-fdc35d751661
    status: accepted
    candidate_group_id: 1bb87db5-2722-4df2-bd84-8c3691b80aa2
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/**
---

# Encapsulating Service Transaction Boundaries with @openstatus/db: Service Boundary Handlers Coordinating Multi Entity

Scope: applies to `packages/services/src/**`.

These rules are ALWAYS ACTIVE for service definition modules performing business logic, entity orchestration, or persistence operations spanning multiple relational entities, join tables, or cascading lifecycle states.

### Rules

- **R-SVC-001** MUST: Service boundary handlers coordinating multi-entity state mutations or cascading updates MUST encapsulate all database mutations within an atomic transaction boundary provided by @openstatus/db.

### Verify

```bash
# Discover the workspace test execution script from the repository manifest and run test suites covering service boundaries.
# Execute the project static analysis and type checking workflow to verify type compatibility across service handlers and database schema definitions.
```

**Accept when:**
- All service boundary operations managing multi-entity mutations execute within atomic transactions without orphaned join-table records.
- Verification suites and static analysis pass without type or transaction isolation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks and peer code reviews enforce these transactional boundaries.
</enforcement>