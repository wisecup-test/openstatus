---
version: 2
name: cross-cutting-monitor-creation-update-procedures-valid-d18f
description: "Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Monitor Creation Update Procedures Validate Assertion"
metadata:
  actual:
    adr_id: d18fd3c6-eb76-4e5e-a972-19e48d78b44c
    status: accepted
    candidate_group_id: 9ecd70cc-9e58-4141-bf80-c73b1ea4168f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Monitor Creation Update Procedures Validate Assertion

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AST-001** MUST: Monitor creation and update procedures MUST validate assertion arrays using assertionUnion or equivalent standardized schemas exported by @openstatus/assertions.

### Verify

```bash
# Discover workspace static analysis and type verification script from the repository manifest and execute it
# Discover and run the project test suite covering API router assertion validation and checker execution workflows
```

**Accept when:**
- All API endpoints accepting assertion inputs validate payloads strictly using schemas exported by @openstatus/assertions without type errors.
- Repository verification procedures complete with zero schema validation failures across all router test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checks and code reviews verify workspace dependency imports and schema usage, and contributions introducing local assertion schemas are rejected.
</enforcement>