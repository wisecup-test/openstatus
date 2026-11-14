---
version: 2
name: cross-cutting-internal-service-execution-logic-consume-0e8f
description: "Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Internal Service Execution Logic Consume Assertion"
metadata:
  actual:
    adr_id: 0e8f0c41-ec42-4eae-b2b9-f9656fe7fc16
    status: accepted
    candidate_group_id: 468c0ac4-a610-419a-85b8-fa4ec46ef6f8
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Internal Service Execution Logic Consume Assertion

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for monitor service schema definitions and monitor execution services that define or process health check assertions.

### Rules

- **R-ASSERT-001** SHOULD: Internal service execution logic SHOULD consume assertion models directly from the assertion module when verifying target status or validating monitor job payloads.

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