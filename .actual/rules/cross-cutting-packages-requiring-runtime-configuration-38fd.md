---
version: 2
name: cross-cutting-packages-requiring-runtime-configuration-38fd
description: "Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Packages Requiring Runtime Configuration Define Validate"
metadata:
  actual:
    adr_id: 38fd52f8-b96e-439c-b677-58ed7dfff0ef
    status: accepted
    candidate_group_id: e3c50a70-1613-41af-b5dc-dedbf511048f
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
---

# Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Packages Requiring Runtime Configuration Define Validate

Scope: applies to `packages/notifications/email/**`.

These rules are ALWAYS ACTIVE for all packages, services, and modules that require runtime configuration, API credentials, or environment-dependent operational modes.

### Rules

- **R-ENV-001** MUST: Packages requiring runtime configuration MUST define and validate their environment schemas using @t3-oss/env-core at package initialization.
- **R-ENV-002** MUST: Define all server-side secrets and client-accessible environment variables in a single dedicated configuration module per package.
- **R-ENV-003** MUST: Export the validated configuration object as the sole access point for environment values across the package.

### Verify

```bash
# Discover and run the project typecheck script from the project manifest
# Discover and run the project test script from the project manifest with sample environment values
# Discover and run the project linter to confirm direct process env access is restricted
```

**Accept when:**
- All environment variable access within the package resolves to strongly typed schema properties without compilation errors.
- Initialization fails fast with descriptive schema validation errors when required environment variables are unset.
- No direct references to raw process environment variables exist in package business logic outside the dedicated environment module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration checks will enforce static typechecking and linting suites on every pull request.
</enforcement>