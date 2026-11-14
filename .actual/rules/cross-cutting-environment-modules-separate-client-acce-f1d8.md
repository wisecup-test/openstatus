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

### Verify

```bash
# Discover and run the project typecheck script from the project manifest
# Discover and run the project test script with sample environment values
# Discover and run the project linter to confirm restricted raw process environment variable access
```

**Accept when:**
- All environment variable access within the package resolves to strongly typed schema properties without compilation errors.
- Initialization fails fast with descriptive schema validation errors when required environment variables are unset.
- No direct references to raw process environment variables exist in package business logic outside the dedicated environment module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>