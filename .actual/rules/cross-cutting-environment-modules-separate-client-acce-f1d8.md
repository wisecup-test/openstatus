---
version: 2
name: cross-cutting-environment-modules-separate-client-acce-f1d8
description: "Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Environment Modules Separate Client Accessible Runtime"
metadata:
  actual:
    adr_id: f1d86726-c887-4259-b604-372e514cd775
    status: accepted
    candidate_group_id: e3c50a70-1613-41af-b5dc-dedbf511048f
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
---

# Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Environment Modules Separate Client Accessible Runtime

Scope: applies to `packages/notifications/email/**`.

These rules are ALWAYS ACTIVE for packages, services, and modules that require runtime configuration, API credentials, or environment-dependent operational modes.

### Rules

- **R-ENV-001** SHOULD: Environment modules SHOULD separate client-accessible runtime configuration from server-only secrets within the schema declaration.
- **R-ENV-002** MANDATORY: Define all server-side secrets and client-accessible environment variables in a single dedicated configuration module per package.
- **R-ENV-003** MANDATORY: Export the validated configuration object as the sole access point for environment values across the package.
- **R-ENV-004** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact for exact resolved version, look up official docs for that exact version, confirm API existence, re-run per dependency at point of use.

### Verify

```bash
# Discover and run the project typecheck script from the project manifest
# to verify that all environment variable accesses resolve against the validated configuration object.
# (Substitute the actual typecheck command derived from package.json, e.g. npm run typecheck or pnpm check)

# Discover and run the project test script from the project manifest with sample environment values
# to verify startup schema validation passes.
# (Substitute the actual test command derived from package.json)

# Discover and run the project linter to confirm that direct access to process environment variables
# is restricted to configuration modules.
# (Substitute the actual lint command derived from package.json)
```

**Accept when:**
- All environment variable access within the package resolves to strongly typed schema properties without compilation errors.
- Initialization fails fast with descriptive schema validation errors when required environment variables are unset.
- No direct references to raw process environment variables exist in package business logic outside the dedicated environment module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>