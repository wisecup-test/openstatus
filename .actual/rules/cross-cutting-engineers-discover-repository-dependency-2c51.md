---
version: 2
name: cross-cutting-engineers-discover-repository-dependency-2c51
description: "Environment Variable Schema Validation with @t3-oss/env-core: Engineers Discover Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: 2c51c2ff-6253-4e2b-96c3-93f1b6f835c7
    status: accepted
    candidate_group_id: 2c36a9ad-0737-47e4-90e0-574d2377a7a5
    evidence:
      source: deep
  scope:
    root: packages/db
---

# Environment Variable Schema Validation with @t3-oss/env-core: Engineers Discover Repository Dependency Resolution Artifact

Scope: applies to `packages/db/**`.

These rules are ALWAYS ACTIVE for configuration management and environment variable access within backend and database workspace packages.

### Rules

- **R-ENV-001** MUST: Engineers MUST discover the repository dependency resolution artifact to resolve and verify the exact locked version of @t3-oss/env-core and zod before implementing or updating environment schemas.

### Verify

```bash
# Discover and run the project's static analysis and type verification suite to confirm that all references to environment variables utilize the validated configuration object.
# Discover and run the repository test suite to verify that invalid or missing environment configurations trigger module-load validation errors as expected.
```

**Accept when:**
- Static verification confirms zero unvalidated references to raw process environment variables within the package boundary.
- Module initialization successfully parses valid environment variables and throws descriptive validation errors when required variables are absent or invalid.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>