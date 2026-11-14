---
version: 2
name: cross-cutting-environment-schemas-define-distinct-serv-cb2f
description: "Adoption of @t3-oss/env-core for Environment Variable Validation: Environment Schemas Define Distinct Server Client"
metadata:
  actual:
    adr_id: cb2f1f8f-a8b3-45fa-b4f2-fd64d008dc84
    status: accepted
    candidate_group_id: cadf5e56-4866-48a6-ab51-36a4a0721d5d
    evidence:
      source: deep
  scope:
    root: apps/screenshot-service
    paths:
      - src/**
---

# Adoption of @t3-oss/env-core for Environment Variable Validation: Environment Schemas Define Distinct Server Client

Scope: applies to `apps/screenshot-service/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: Environment schemas SHOULD define distinct server and client scopes when exposing configuration to prevent sensitive environment variables from leaking across runtime boundaries.

### Verify

```bash
# Discover and run the project verification script from the repository manifest
# and execute the test runner to validate environment configuration parsing.
# Discover the static analysis and type checking script to verify environment schema type inference and variable references across modules.
```

**Accept when:**
- The verification test suite completes successfully without reporting environment schema parsing errors.
- The static type checker passes with zero diagnostics on exported environment configuration properties.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static type checking, linting pipelines, and test suites enforce these requirements.
</enforcement>