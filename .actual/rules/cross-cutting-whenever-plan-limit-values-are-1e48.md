---
version: 2
name: cross-cutting-whenever-plan-limit-values-are-1e48
description: "Adoption of workspaces/validation Module for Plan Schema Validation: Whenever Plan Limit Values Are Processed"
metadata:
  actual:
    adr_id: 1e4805b8-b74e-42ae-b5b2-be79589b2857
    status: accepted
    candidate_group_id: f010b973-6aa7-44f9-9802-0a4ddfc028ad
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/plan/**
---

# Adoption of workspaces/validation Module for Plan Schema Validation: Whenever Plan Limit Values Are Processed

Scope: applies to `packages/db/src/schema/plan/**`.

These rules are ALWAYS ACTIVE for all database schema modules defining plan limits, configuration entities, and related utility operations, and any service or domain layer modifying plan limits and workspace configurations.

### Rules

- **R-VAL-001** MUST: Whenever plan limit values are processed, the application MUST invoke limitsSchema.parse to validate incoming limits against schema constraints prior to persistence or configuration assignment.

### Verify

```bash
# Discover and run the project static type checker and linter scripts configured in the repository manifest
# Execute the repository test suite targeted at schema configuration and validation utilities
```

**Accept when:**
- Static type analysis and schema validation tests compile and execute without errors.
- Test suites asserting valid and invalid plan limit structures pass successfully against the validation contract.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>