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

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-DB-001** MUST_NOT: Service boundary handlers MUST NOT perform cascading deletions or join-table mutations outside of a shared transactional client context.

### Verify

```bash
# Discover and run test suites covering service boundaries as defined in the repository manifest
# Execute project static analysis and type checking workflow
```

**Accept when:**
- All service boundary operations managing multi-entity mutations execute within atomic transactions without orphaned join-table records.
- Verification suites and static analysis pass without type or transaction isolation errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>