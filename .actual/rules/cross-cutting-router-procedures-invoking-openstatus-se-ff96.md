---
version: 2
name: cross-cutting-router-procedures-invoking-openstatus-se-ff96
description: "Adoption of @openstatus/services Internal Service Library for API Operations: Router Procedures Invoking Openstatus Services Functions"
metadata:
  actual:
    adr_id: ff961f16-1b1e-494e-a102-eccc359608e3
    status: accepted
    candidate_group_id: 325c1754-ad4f-40ef-aa1d-165da2308b07
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services Internal Service Library for API Operations: Router Procedures Invoking Openstatus Services Functions

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API router procedure handlers and domain service integrations across workspace packages interacting with core entities.

### Rules

- **R-OPENSTATUS-001** MUST: Router procedures invoking @openstatus/services functions MUST transform incoming transport context into standard service context using toServiceCtx before passing execution parameters to domain service methods.

### Verify

```bash
# Discover and execute the test runner script covering API routes and service adapters
# Discover and execute workspace static analysis and type checking scripts
```

**Accept when:**
- All route procedure calls successfully delegate domain logic to @openstatus/services through service context and error adaptation.
- Type checking and static verification across all API modules pass without unresolved imports or structural boundary violations.
- Integration and unit test suites covering API router endpoints pass with zero unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Compliance is verified by automated static analysis, type verification, and architectural review.
</enforcement>