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

These rules are ALWAYS ACTIVE for all public and protected procedure definitions accepting parameters from external callers, and runtime parsing of dynamic configuration objects and deserialized entity attributes.

### Rules

- **R-ZOD-001** MUST: All ingress procedure endpoints and route handlers MUST enforce runtime boundary validation using Zod schemas on all input parameters before invoking downstream service logic.

### Verify

```bash
# Discover and execute the repository build, type verification, test runner, and static analysis linter from the project manifest
# Example discovery sequence:
# 1. Inspect package.json for build/typecheck/test/lint scripts
# 2. Run the discovered type check script to verify schema compilation across all procedure definitions
# 3. Run the discovered automated test suite to validate that malformed input payloads are rejected and valid payloads are accepted
# 4. Run the discovered linter to ensure all procedure definitions declare input validation contracts without bypasses
```

**Accept when:**
- All public and protected procedure definitions declare explicit input validation schemas prior to invoking service handlers.
- Type checking and automated test suites confirm schema compilation and successful rejection of malformed payloads.
- Boundary parser rejections return structured client errors without propagating unvalidated state to domain functions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipelines, peer review protocols, and automated static analysis enforce that all ingress procedure endpoints and route handlers enforce runtime boundary validation using Zod schemas.
</enforcement>