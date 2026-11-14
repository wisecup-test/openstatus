---
version: 2
name: cross-cutting-configuration-schema-evaluation-fail-fas-d3d2
description: "Environment Variable Schema Validation with @t3-oss/env-core: Configuration Schema Evaluation Fail Fast Module"
metadata:
  actual:
    adr_id: d3d2312d-1bc9-43ab-acf2-453c94001d86
    status: accepted
    candidate_group_id: 2c36a9ad-0737-47e4-90e0-574d2377a7a5
    evidence:
      source: deep
  scope:
    root: packages/db
---

# Environment Variable Schema Validation with @t3-oss/env-core: Configuration Schema Evaluation Fail Fast Module

Scope: applies to `packages/db/**`.

These rules are ALWAYS ACTIVE for configuration management and environment variable access within backend and database workspace packages.

### Rules

- **R-ENV-001** MUST: Configuration schema evaluation MUST fail fast at module load time if any required environment variable is missing, undefined, or fails schema validation.

### Verify

```bash
# Discover and run the project's static analysis and type verification suite to confirm that all references to environment variables utilize the validated configuration object.
# Discover and run the repository test suite to verify that invalid or missing environment configurations trigger module-load validation errors as expected.
```

**Accept when:**
- Static verification confirms zero unvalidated references to raw process environment variables within the package boundary.
- Module initialization successfully parses valid environment variables and throws descriptive validation errors when required variables are absent or invalid.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing direct process environment access or unvalidated configuration reads are blocked until reconciled with the environment schema.
</enforcement>