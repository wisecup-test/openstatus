---
version: 2
name: cross-cutting-untrusted-network-configuration-payloads-7885
description: "Zod Schema Validation for Boundary Inputs and Form State: Untrusted Network Configuration Payloads Validated Non"
metadata:
  actual:
    adr_id: 788500f6-4c34-4792-a41b-a8d075685ff0
    status: accepted
    candidate_group_id: f6e99943-2e18-4231-91cb-1260300e20ee
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/**
---

# Zod Schema Validation for Boundary Inputs and Form State: Untrusted Network Configuration Payloads Validated Non

Scope: applies to `apps/status-page/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ZOD-001** MUST: Untrusted network and configuration payloads MUST be validated using non-throwing schema parsing methods such as safeParse, with validation errors explicitly handled before data consumption.

### Verify

```bash
# Discover and execute the repository automated test suite to confirm schema validation rules
# Execute the project typechecking command to verify type alignment between declared schemas and downstream contracts
# Run the repository linting and static analysis tool to ensure compliance with schema parsing standards
```

**Accept when:**
- All schema validation test suites complete with zero failures across boundary handlers and form components.
- The repository typechecking command succeeds without type mismatches between validated schemas and downstream consumer contracts.
- Untrusted inputs arriving at boundary endpoints and user interface forms are verified to pass through schema parsing before processing.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing unvalidated external inputs or untyped assertions at boundaries are blocked.
</enforcement>