---
version: 2
name: cross-cutting-ingress-procedure-endpoints-route-handle-1d1b
description: "Zod Runtime Schema Validation for API Procedure Boundaries: Ingress Procedure Endpoints Route Handlers Enforce"
metadata:
  actual:
    adr_id: 1d1bc296-168a-4bad-a2ff-5ffde342ced8
    status: accepted
    candidate_group_id: 0cbb6056-fd7d-4539-b46b-da94fc5fa10f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# Zod Runtime Schema Validation for API Procedure Boundaries: Ingress Procedure Endpoints Route Handlers Enforce

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: All ingress procedure endpoints and route handlers MUST enforce runtime boundary validation using Zod schemas on all input parameters before invoking downstream service logic.

### Verify

```bash
# Discover the repository build and type verification script from the project manifest and execute it
# Discover and execute the repository automated test runner
# Discover and run the project static analysis linter
```

**Accept when:**
- All public and protected procedure definitions declare explicit input validation schemas prior to invoking service handlers.
- Type checking and automated test suites confirm schema compilation and successful rejection of malformed payloads.
- Boundary parser rejections return structured client errors without propagating unvalidated state to domain functions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>