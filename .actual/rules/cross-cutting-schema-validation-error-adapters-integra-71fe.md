---
version: 2
name: cross-cutting-schema-validation-error-adapters-integra-71fe
description: "Adoption of base-error Internal Module for Structured Error Definitions: Schema Validation Error Adapters Integrating External"
metadata:
  actual:
    adr_id: 71fed008-ebbd-46da-b4c7-118a505da5b6
    status: accepted
    candidate_group_id: 4689aee9-392e-481e-85d5-49d67acc913c
    evidence:
      source: deep
  scope:
    root: packages/error
    paths:
      - src/**
---

# Adoption of base-error Internal Module for Structured Error Definitions: Schema Validation Error Adapters Integrating External

Scope: applies to `packages/error/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Schema validation error adapters integrating external schema validation libraries MUST map external validation failures into the internal base-error representation.

### Verify

\`\`\`bash
# Discover the project test execution script from the workspace dependency manifest and execute test validation across error modules.
# Discover and execute the project type-checking and linting verification scripts to ensure compliance with base-error interface constraints.
\`\`\`

**Accept when:**
- All specialized error classes inherit from or encapsulate the internal base-error module and compile without interface mismatch.
- Automated verification suites confirm consistent error code mapping and metadata serialization across all specialized error definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>