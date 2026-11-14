---
version: 2
name: cross-cutting-environment-configuration-schemas-valida-2f6c
description: "Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Environment Configuration Schemas Validate Declared Runtime"
metadata:
  actual:
    adr_id: 2f6cf695-f015-4032-88a6-00fc7ae8649c
    status: accepted
    candidate_group_id: e3c50a70-1613-41af-b5dc-dedbf511048f
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
---

# Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Environment Configuration Schemas Validate Declared Runtime

Scope: applies to `packages/notifications/email/**`.

These rules are ALWAYS ACTIVE for packages, services, and modules that require runtime configuration, API credentials, or environment-dependent operational modes.

### Rules

- **R-ENV-001** MUST: Environment configuration schemas MUST validate all declared runtime keys, secrets, and operational modes with explicit schema validators prior to export.

### Verify

```bash
# Discover and run the project typecheck script from the project manifest
# Discover and run the project test script from the project manifest with sample environment values
# Discover and run the project linter to confirm that direct access to process environment variables is restricted to configuration modules
```

**Accept when:**
- All environment variable access within the package resolves to strongly typed schema properties without compilation errors.
- Initialization fails fast with descriptive schema validation errors when required environment variables are unset.
- No direct references to raw process environment variables exist in package business logic outside the dedicated environment module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>