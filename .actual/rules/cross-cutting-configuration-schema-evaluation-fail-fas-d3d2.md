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
- **R-ENV-002** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ENV-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: (1) Find dependency manifest, (2) Identify build tool, (3) Inspect repository lock/resolution artifact, (4) Look up official documentation for exact version, (5) Confirm every API/class/function exists in that exact version, (6) Re-run steps 3-5 per dependency at point of use.
- **R-ENV-004** MUST: Define all package-level environment variables within a dedicated configuration definition utilizing the project's environment validation library and export the parsed object as the sole access point.
- **R-ENV-005** MUST: Apply specific validation schemas for each variable to enforce format correctness, string non-emptiness, and domain-specific configuration constraints.

### Verify

```bash
# Discover and run the project's static analysis and type verification suite
# to confirm that all references to environment variables utilize the validated configuration object.
# Discover and run the repository test suite to verify that invalid or missing 
# environment configurations trigger module-load validation errors as expected.
```

**Accept when:**
- Static verification confirms zero unvalidated references to raw process environment variables within the package boundary.
- Module initialization successfully parses valid environment variables and throws descriptive validation errors when required variables are absent or invalid.

<enforcement>
Claude Code MUST NOT skip or defer verification. Direct process environment access or unvalidated configuration reads are blocked until reconciled with the environment schema.
</enforcement>