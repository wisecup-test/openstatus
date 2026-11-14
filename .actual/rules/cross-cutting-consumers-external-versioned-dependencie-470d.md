---
version: 2
name: cross-cutting-consumers-external-versioned-dependencie-470d
description: "Adoption of workspaces/validation Module for Plan Schema Validation: Consumers External Versioned Dependencies Discover Ecosystem"
metadata:
  actual:
    adr_id: 470d3d13-a34d-4ea0-ac63-ef5f9b3d970d
    status: accepted
    candidate_group_id: f010b973-6aa7-44f9-9802-0a4ddfc028ad
    evidence:
      source: deep
  scope:
    root: packages/db
    paths:
      - src/schema/plan/**
---

# Adoption of workspaces/validation Module for Plan Schema Validation: Consumers External Versioned Dependencies Discover Ecosystem

Scope: applies to `packages/db/src/schema/plan/**`.

These rules are ALWAYS ACTIVE for all database schema modules defining plan limits, configuration entities, related utility operations, and any service or domain layer modifying plan limits and workspace configurations.

### Rules

- **R-VAL-001** MUST: Consumers of external or versioned dependencies MUST discover the ecosystem lock file and verify the resolved dependency version before implementing schema validation bindings.

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