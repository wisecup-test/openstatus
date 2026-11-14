---
version: 2
name: cross-cutting-monitor-configuration-schemas-define-ass-2ebd
description: "Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Monitor Configuration Schemas Define Assertions Array"
metadata:
  actual:
    adr_id: 2ebd3fe2-fc64-47ae-a8e6-f242b01042fd
    status: accepted
    candidate_group_id: 468c0ac4-a610-419a-85b8-fa4ec46ef6f8
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Monitor Configuration Schemas Define Assertions Array

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for monitor service schema definitions and monitor execution services that define or process health check assertions.

### Rules

- **R-OPENSTATUS-001** MUST: Monitor configuration schemas MUST define assertions as an array of assertion objects sourced from the shared assertion module with an empty array default.

### Verify

```bash
# Discover and execute the project type verification script from the project manifest
# Discover and run the project test suite to verify monitor schema validation and assertion processing
```

**Accept when:**
- All monitor service schemas validate assertion arrays using types defined in the shared assertions module.
- Repository type verification and automated test suites execute with zero failures across service modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated type checking and static analysis enforce compliance during continuous integration pipelines.
</enforcement>