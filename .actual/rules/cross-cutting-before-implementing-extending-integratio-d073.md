---
version: 2
name: cross-cutting-before-implementing-extending-integratio-d073
description: "Adoption of @openstatus/services Internal Service Library for API Operations: Before Implementing Extending Integration Openstatus Services"
metadata:
  actual:
    adr_id: d073a08d-8d37-4ed1-84e6-f3e93c7ad0bc
    status: accepted
    candidate_group_id: 325c1754-ad4f-40ef-aa1d-165da2308b07
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services Internal Service Library for API Operations: Before Implementing Extending Integration Openstatus Services

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all API router procedure handlers and domain service integrations interacting with core entities.

### Rules

- **R-OPENSTATUS-001** MUST: Before implementing or extending integration with @openstatus/services or any versioned dependency, developers MUST inspect the workspace dependency manifest and repository lock artifact to verify the exact resolved version and adhere to its published API contract.

### Verify

```bash
# Discover and execute the test runner script from the configuration manifest
# Discover and execute workspace static analysis and type checking scripts
```

**Accept when:**
- All route procedure calls successfully delegate domain logic to @openstatus/services through service context and error adaptation.
- Type checking and static verification across all API modules pass without unresolved imports or structural boundary violations.
- Integration and unit test suites covering API router endpoints pass with zero unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is enforced by automated static analysis, type verification, and architectural pull request reviews.
</enforcement>