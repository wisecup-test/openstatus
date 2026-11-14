---
version: 2
name: cross-cutting-execution-verification-logic-evaluate-as-21f6
description: "Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Execution Verification Logic Evaluate Assertion Outcomes"
metadata:
  actual:
    adr_id: 21f67304-4729-45b5-9e56-4da915341468
    status: accepted
    candidate_group_id: 9ecd70cc-9e58-4141-bf80-c73b1ea4168f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Execution Verification Logic Evaluate Assertion Outcomes

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-AST-001** MUST: Execution verification logic MUST evaluate assertion outcomes against the structured success property defined by the @openstatus/assertions interface contract.

### Verify

```bash
# Discover the workspace static analysis and type verification script from the repository manifest and execute it
# Discover and run the project test suite covering API router assertion validation and checker execution workflows
```

**Accept when:**
- All API endpoints accepting assertion inputs validate payloads strictly using schemas exported by @openstatus/assertions without type errors.
- Repository verification procedures complete with zero schema validation failures across all router test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration type checks verify workspace dependency imports and schema usage, and code reviews ensure no ad-hoc assertion schemas are defined in router layers.
</enforcement>