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

These rules are ALWAYS ACTIVE for packages, services, and modules that require runtime configuration, API credentials, or environment-dependent operational modes.

### Rules

- **R-ENV-001** MUST: Packages requiring runtime configuration MUST define and validate their environment schemas using @t3-oss/env-core at package initialization.
- **R-ENV-002** MANDATORY (DISCOVERY POLICY): The consumer MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ENV-003** MANDATORY (LOCK-VERSION GROUNDING): Before writing code that uses a versioned library, execute in order: find dependency manifest, identify build tool, inspect repository lock/resolution artifact for exact version, look up official documentation for that exact version, confirm every API/class/function exists in that version, and re-run for version-sensitive behavior at point of use.
- **R-ENV-004** MUST: Define all server-side secrets and client-accessible environment variables in a single dedicated configuration module per package.
- **R-ENV-005** MUST: Export the validated configuration object as the sole access point for environment values across the package.

### Verify

```bash
# Discover and run the project typecheck script from the project manifest
# to verify that all environment variable accesses resolve against the validated configuration object.
# (Example: pnpm typecheck or npm run typecheck - derived from project manifest)

# Discover and run the project test script from the project manifest with sample environment values
# to verify startup schema validation passes.
# (Example: pnpm test or npm test - derived from project manifest)

# Discover and run the project linter to confirm that direct access to process
# environment variables is restricted to configuration modules.
# (Example: pnpm lint or npm run lint - derived from project manifest)
```

**Accept when:**
- All environment variable access within the package resolves to strongly typed schema properties without compilation errors.
- Initialization fails fast with descriptive schema validation errors when required environment variables are unset.
- No direct references to raw process environment variables exist in package business logic outside the dedicated environment module.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing direct raw environment variable reads or undeclared configuration properties will fail continuous integration and be blocked from merging.
</enforcement>