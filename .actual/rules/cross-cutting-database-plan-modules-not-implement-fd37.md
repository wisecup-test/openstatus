---
version: 2
name: cross-cutting-database-plan-modules-not-implement-fd37
description: "Adoption of workspaces/validation Module for Plan Schema Validation: Database Plan Modules Not Implement Hoc"
metadata:
  actual:
    adr_id: fd37a497-7d5e-4813-821e-296033948a9b
    status: accepted
    candidate_group_id: f010b973-6aa7-44f9-9802-0a4ddfc028ad
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/plan/**
---

# Adoption of workspaces/validation Module for Plan Schema Validation: Database Plan Modules Not Implement Hoc

Scope: applies to `packages/db/src/schema/plan/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-VAL-001** MUST_NOT: Database plan modules MUST NOT implement ad-hoc inline limit validation checks or duplicate schema definitions outside of the designated validation module.

### Verify

```bash
# Discover and run the project static type checker and linter scripts configured in the repository manifest.
# Execute the repository test suite targeted at schema configuration and validation utilities.
```

**Accept when:**
- Static type analysis and schema validation tests compile and execute without errors.
- Test suites asserting valid and invalid plan limit structures pass successfully against the validation contract.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>