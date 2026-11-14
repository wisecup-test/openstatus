---
version: 2
name: cross-cutting-monitor-service-components-model-validat-a88f
description: "Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Monitor Service Components Model Validate Evaluate"
metadata:
  actual:
    adr_id: a88f53bf-7bca-4ba3-a74c-d1efbf7dcd9b
    status: accepted
    candidate_group_id: 468c0ac4-a610-419a-85b8-fa4ec46ef6f8
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Monitor Service Components Model Validate Evaluate

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-MON-001** MUST: Monitor service components MUST model, validate, and evaluate monitor health assertions using the @openstatus/assertions module rather than declaring ad-hoc validation primitives or inline assertion evaluation logic.

### Verify

```bash
# Discover the repository type verification script from the project manifest and execute it to confirm type compatibility with the assertions module.
# Discover and run the project test suite to verify monitor schema validation and assertion processing.
```

**Accept when:**
- All monitor service schemas validate assertion arrays using types defined in the shared assertions module.
- Repository type verification and automated test suites execute with zero failures across service modules.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>