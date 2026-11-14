---
version: 2
name: cross-cutting-plan-configuration-limit-mutation-proced-d18c
description: "Adoption of workspaces/validation Module for Plan Schema Validation: Plan Configuration Limit Mutation Procedures Import"
metadata:
  actual:
    adr_id: d18c316c-e06e-4c27-8c15-0323aa280fa3
    status: accepted
    candidate_group_id: f010b973-6aa7-44f9-9802-0a4ddfc028ad
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/plan/**
---

# Adoption of workspaces/validation Module for Plan Schema Validation: Plan Configuration Limit Mutation Procedures Import

Scope: applies to `packages/db/src/schema/plan/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-VAL-001** MUST: Plan configuration and limit mutation procedures MUST import and enforce schema structures defined in the workspaces/validation internal module.

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