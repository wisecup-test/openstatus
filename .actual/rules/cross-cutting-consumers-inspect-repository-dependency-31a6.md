---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-31a6
description: "Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Consumers Inspect Repository Dependency Manifest Lock"
metadata:
  actual:
    adr_id: 31a6e47b-43a2-44fe-9635-b87855639fc2
    status: accepted
    candidate_group_id: 468c0ac4-a610-419a-85b8-fa4ec46ef6f8
    evidence:
      source: deep
  scope:
    root: packages/services
    paths:
      - src/monitor/**
---

# Adoption of @openstatus/assertions for Monitor Assertion Modeling and Evaluation: Consumers Inspect Repository Dependency Manifest Lock

Scope: applies to `packages/services/src/monitor/**`.

These rules are ALWAYS ACTIVE for monitor service schema definitions and monitor execution services that define or process health check assertions.

### Rules

- **R-AST-001** MUST: Consumers MUST inspect the repository dependency manifest and lock artifact to resolve the exact version of the assertions library before implementing or updating assertion integrations.

### Verify

```bash
# Discover the repository type verification script from the project manifest and execute it to confirm type compatibility with the assertions module.
# Discover and run the project test suite to verify monitor schema validation and assertion processing.
```

**Accept when:**
- All monitor service schemas validate assertion arrays using types defined in the shared assertions module.
- Repository type verification and automated test suites execute with zero failures across service modules.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>