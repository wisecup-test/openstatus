---
version: 2
name: cross-cutting-service-modules-not-declare-duplicate-6fcb
description: "Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Service Modules Not Declare Duplicate Conflicting"
metadata:
  actual:
    adr_id: 6fcb8e73-4c65-4fdf-8176-ba50b4676cb1
    status: accepted
    candidate_group_id: 468c0ac4-a610-419a-85b8-fa4ec46ef6f8
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Service Modules Not Declare Duplicate Conflicting

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for monitor service schema definitions and monitor execution services that define or process health check assertions.

### Rules

- **R-AST-001** MUST_NOT: Service modules MUST NOT declare duplicate or conflicting assertion schema types outside the dedicated assertion module.

### Verify

```bash
# Discover and execute the repository type verification script from the project manifest
# Discover and run the project test suite
```

**Accept when:**
- All monitor service schemas validate assertion arrays using types defined in the shared assertions module.
- Repository type verification and automated test suites execute with zero failures across service modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Ad-hoc assertion schemas or bypasses of the shared assertions module are strictly blocked.
</enforcement>