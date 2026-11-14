---
version: 2
name: cross-cutting-router-procedure-invocations-domain-serv-3e00
description: "Adoption of @openstatus/services Internal Service Library for API Operations: Router Procedure Invocations Domain Service Operations"
metadata:
  actual:
    adr_id: 3e0087a4-8edc-427e-980d-20a069de4bbd
    status: accepted
    candidate_group_id: 325c1754-ad4f-40ef-aa1d-165da2308b07
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services Internal Service Library for API Operations: Router Procedure Invocations Domain Service Operations

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ROUTER-001** MUST: Router procedure invocations of domain service operations MUST catch exceptions and transform domain failures into standardized transport errors using toTRPCError or throwFromException.

### Verify

```bash
# Discover the workspace static analysis and type checking scripts from the configuration manifest and execute validation across all affected packages.
npm run typecheck
# Discover the repository test runner script from the configuration manifest and execute the full test suite covering API routes and service adapters.
npm test
```

**Accept when:**
- All route procedure calls successfully delegate domain logic to @openstatus/services through service context and error adaptation.
- Type checking and static verification across all API modules pass without unresolved imports or structural boundary violations.
- Integration and unit test suites covering API router endpoints pass with zero unhandled exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>