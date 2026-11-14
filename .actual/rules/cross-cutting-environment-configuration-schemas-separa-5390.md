---
version: 2
name: cross-cutting-environment-configuration-schemas-separa-5390
description: "Environment Variable Schema Validation with @t3-oss/env-core: Environment Configuration Schemas Separate Server Only"
metadata:
  actual:
    adr_id: 53905af0-21ba-40ab-a48a-1b1f144fd323
    status: accepted
    candidate_group_id: 2c36a9ad-0737-47e4-90e0-574d2377a7a5
    evidence:
      source: deep
  scope:
    root: packages/db
---

# Environment Variable Schema Validation with @t3-oss/env-core: Environment Configuration Schemas Separate Server Only

Scope: applies to `packages/db/**`.

These rules are ALWAYS ACTIVE for all configuration management and environment variable access within backend and database workspace packages.

### Rules

- **R-ENV-001** SHOULD: Environment configuration schemas SHOULD separate server-only secrets from client-accessible configuration variables within the schema definition.
- **R-ENV-002** MANDATORY: Define all package-level environment variables within a dedicated configuration definition utilizing the project's environment validation library and export the parsed object as the sole access point.
- **R-ENV-003** MANDATORY: Apply specific type/format schemas for each environment variable to enforce format correctness, string non-emptiness, and domain-specific configuration constraints.

### Verify

```bash
# Discover and run the project's static analysis and type verification suite to confirm unvalidated references are absent
# Discover and run the repository test suite to verify module-load validation errors on invalid configurations
```

**Accept when:**
- Static verification confirms zero unvalidated references to raw process environment variables within the package boundary.
- Module initialization successfully parses valid environment variables and throws descriptive validation errors when required variables are absent or invalid.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing direct process environment access or unvalidated configuration reads are blocked until reconciled with the environment schema.
</enforcement>