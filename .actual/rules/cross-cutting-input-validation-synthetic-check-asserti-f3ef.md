---
version: 2
name: cross-cutting-input-validation-synthetic-check-asserti-f3ef
description: "Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Input Validation Synthetic Check Assertions Employ"
metadata:
  actual:
    adr_id: f3ef5f1c-d56e-413e-afdc-85b89de9b034
    status: accepted
    candidate_group_id: 9ecd70cc-9e58-4141-bf80-c73b1ea4168f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Input Validation Synthetic Check Assertions Employ

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MUST: Input validation for synthetic check assertions MUST employ the discriminated union schema based on the type discriminator property, utilizing statusAssertion, headerAssertion, textBodyAssertion, jsonBodyAssertion, and recordAssertion definitions from @openstatus/assertions.

### Verify

```bash
# Discover and execute the workspace static analysis and type verification script from the repository manifest
# Discover and run the project test suite covering API router assertion validation and checker execution workflows
```

**Accept when:**
- All API endpoints accepting assertion inputs validate payloads strictly using schemas exported by @openstatus/assertions without type errors.
- Repository verification procedures complete with zero schema validation failures across all router test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>