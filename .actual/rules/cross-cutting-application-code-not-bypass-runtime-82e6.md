---
version: 2
name: cross-cutting-application-code-not-bypass-runtime-82e6
description: "Zod Schema Validation for Boundary Inputs and Form State: Application Code Not Bypass Runtime Schema"
metadata:
  actual:
    adr_id: 82e6589f-0af0-43e7-88aa-8223d50ce739
    status: accepted
    candidate_group_id: f6e99943-2e18-4231-91cb-1260300e20ee
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Zod Schema Validation for Boundary Inputs and Form State: Application Code Not Bypass Runtime Schema

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** SHOULD_NOT: Application code SHOULD_NOT bypass runtime schema parsing by applying type assertions or unchecked casting directly to external inputs.

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