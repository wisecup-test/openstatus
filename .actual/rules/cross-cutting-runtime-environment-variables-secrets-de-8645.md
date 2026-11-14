---
version: 2
name: cross-cutting-runtime-environment-variables-secrets-de-8645
description: "Environment Variable Schema Validation with @t3-oss/env-core: Runtime Environment Variables Secrets Declared Validated"
metadata:
  actual:
    adr_id: 86452ba3-8f39-4afb-8b64-d3e9ed6f19d4
    status: accepted
    candidate_group_id: 2c36a9ad-0737-47e4-90e0-574d2377a7a5
    evidence:
      source: deep
  scope:
    root: packages/db
---

# Environment Variable Schema Validation with @t3-oss/env-core: Runtime Environment Variables Secrets Declared Validated

Scope: applies to `packages/db/**`.

These rules are ALWAYS ACTIVE for configuration management and environment variable access within backend and database workspace packages, and initialization of runtime credentials, database connection parameters, and operational environment flags.

### Rules

- **R-ENV-001** MUST: Runtime environment variables and secrets MUST be declared and validated through an explicit schema using @t3-oss/env-core and zod rather than accessed directly via unvalidated process environment references.

### Verify

```bash
# Discover and run the project's static analysis and type verification suite
# Example discovery:
# pnpm --filter <package> check || npm run check

# Discover and run the repository test suite
# Example discovery:
# pnpm --filter <package> test || npm test
```

**Accept when:**
- Static verification confirms zero unvalidated references to raw process environment variables within the package boundary.
- Module initialization successfully parses valid environment variables and throws descriptive validation errors when required variables are absent or invalid.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>