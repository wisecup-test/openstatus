---
version: 2
name: cross-cutting-schema-validation-errors-halt-applicatio-7d6e
description: "@t3-oss/env-core Environment Variable Schema Validation: Schema Validation Errors Halt Application Initialization"
metadata:
  actual:
    adr_id: 7d6ee663-197a-4d98-8cf6-2d4cc0173634
    status: accepted
    candidate_group_id: 3d0a80d9-ab93-46e2-9085-ac3694326c96
    evidence:
      source: deep
  scope:
    root: packages/analytics
---

# @t3-oss/env-core Environment Variable Schema Validation: Schema Validation Errors Halt Application Initialization

Scope: applies to `packages/analytics/**`.

These rules are ALWAYS ACTIVE for modules and packages that define, export, or consume runtime environment configurations, analytics tokens, or service credentials.

### Rules

- **R-ENV-001** SHOULD: Schema validation errors SHOULD halt application initialization immediately with descriptive diagnostics detailing missing or malformed configuration keys.
- **R-ENV-002** MANDATORY: Construct environment configuration modules by importing the schema generator from the environment core package and declaring type validators using zod for each expected variable.
- **R-ENV-003** MANDATORY: Export the validated configuration object as the single point of entry for consuming modules rather than exposing direct references to runtime environment properties.

### Verify

```bash
# Discover the project test script from the dependency manifest and execute the test runner to verify environment schema parsing.
# Discover and run the project type checking script to ensure exported environment configurations satisfy static type signatures.
# Discover and execute the project linting and boundary verification scripts to confirm no direct accesses to unvalidated runtime environment variables remain.
```

**Accept when:**
- Environment variable validation successfully initializes and parses required configuration entries without throwing runtime schema errors.
- All modules requiring runtime configuration import validated objects rather than accessing raw environment properties directly.
- Type checking and automated test suites pass without validation or typing diagnostics.

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests that introduce direct runtime environment property access or omit schema definitions are blocked from merging.
</enforcement>