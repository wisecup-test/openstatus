---
version: 2
name: cross-cutting-application-import-synthetic-monitoring-fe20
description: "Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Application Import Synthetic Monitoring Assertion Schemas"
metadata:
  actual:
    adr_id: fe2069a7-4b5e-4078-a441-75381d4a3e5c
    status: accepted
    candidate_group_id: 9ecd70cc-9e58-4141-bf80-c73b1ea4168f
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/assertions Module for Synthetic Check Definitions and Validation: Application Import Synthetic Monitoring Assertion Schemas

Scope: applies to `packages/api/src/router/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OPN-001** MUST: The application MUST import synthetic monitoring assertion schemas and validation contracts exclusively from the internal module @openstatus/assertions.

### Verify

```bash
# Discover the workspace static analysis and type verification script from the repository manifest and execute it
# Discover and run the project test suite covering API router assertion validation and checker execution workflows
```

**Accept when:**
- All API endpoints accepting assertion inputs validate payloads strictly using schemas exported by @openstatus/assertions without type errors.
- Repository verification procedures complete with zero schema validation failures across all router test suites.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>