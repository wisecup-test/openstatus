---
version: 2
name: cross-cutting-external-inputs-including-request-parame-f83e
description: "Zod Schema Validation for Boundary Inputs and Form State: External Inputs Including Request Parameters Serialized"
metadata:
  actual:
    adr_id: f83ed11b-73fc-4bac-baa2-1567b31d6e2d
    status: accepted
    candidate_group_id: f6e99943-2e18-4231-91cb-1260300e20ee
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Zod Schema Validation for Boundary Inputs and Form State: External Inputs Including Request Parameters Serialized

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: All external inputs, including request parameters, serialized configuration objects, and interactive form payloads, MUST be validated against declared Zod schemas at runtime boundaries prior to application processing.

### Verify

```bash
# Discover and execute the repository automated test suite to confirm schema validation rules and error branches behave as expected.
# Execute the project typechecking command to verify type alignment between declared schemas and downstream consumer contracts.
# Run the repository linting and static analysis tool to ensure compliance with schema parsing standards across boundary handlers.
```

**Accept when:**
- All schema validation test suites complete with zero failures across boundary handlers and form components.
- The repository typechecking command succeeds without type mismatches between validated schemas and downstream consumer contracts.
- Untrusted inputs arriving at boundary endpoints and user interface forms are verified to pass through schema parsing before processing.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>