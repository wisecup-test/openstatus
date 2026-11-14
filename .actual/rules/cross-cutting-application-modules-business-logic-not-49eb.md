---
version: 2
name: cross-cutting-application-modules-business-logic-not-49eb
description: "Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Application Modules Business Logic Not Directly"
metadata:
  actual:
    adr_id: 49eb004f-587e-4fcb-9aed-f77b0cc3d4c5
    status: accepted
    candidate_group_id: e3c50a70-1613-41af-b5dc-dedbf511048f
    evidence:
      source: deep
  scope:
    root: packages/notifications/email
---

# Adopt @t3-oss/env-core for Typed Runtime Environment Variable Validation: Application Modules Business Logic Not Directly

Scope: applies to `packages/notifications/email/**`.

These rules are ALWAYS ACTIVE for packages, services, and modules that require runtime configuration, API credentials, or environment-dependent operational modes.

### Rules

- **R-ENV-001** MUST_NOT: Application modules and business logic MUST NOT directly read raw process environment variables outside the dedicated environment configuration module.
- **R-ENV-002** MUST: Define all server-side secrets and client-accessible environment variables in a single dedicated configuration module per package.
- **R-ENV-003** MUST: Export the validated configuration object as the sole access point for environment values across the package.

### Verify

```bash
# Discover and run the project typecheck script from the project manifest
# Discover and run the project test script from the project manifest with sample environment values
# Discover and run the project linter to confirm direct access to process environment variables is restricted
```

**Accept when:**
- All environment variable access within the package resolves to strongly typed schema properties without compilation errors.
- Initialization fails fast with descriptive schema validation errors when required environment variables are unset.
- No direct references to raw process environment variables exist in package business logic outside the dedicated environment module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>