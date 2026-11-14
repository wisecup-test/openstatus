---
version: 2
name: cross-cutting-file-transformation-build-scripts-perfor-bb1d
description: "Adoption of Node.js Core Protocol Modules in Automation Scripts: File Transformation Build Scripts Perform Synchronous"
metadata:
  actual:
    adr_id: bb1db619-c683-49ae-b56f-475831fd9b1c
    status: accepted
    candidate_group_id: 11334636-f0f6-4dc3-8271-44dab7fdce77
    evidence:
      source: deep
  scope:
    root: packages/ui
    paths:
      - scripts/**
---

# Adoption of Node.js Core Protocol Modules in Automation Scripts: File Transformation Build Scripts Perform Synchronous

Scope: applies to `packages/ui/scripts/**`.

These rules are ALWAYS ACTIVE for build scripts, asset transformation utilities, and workspace code generation scripts across the monorepo.

### Rules

- **R-ACT-001** SHOULD: File transformation and build scripts SHOULD perform synchronous read operations using readFileSync for configuration loading and validate parsed JSON structures immediately after extraction.
- **R-ACT-002** MANDATORY: Declare module imports using the explicit protocol specifier prefix to disambiguate built-in runtime modules from external dependencies when authoring workspace automation routines.
- **R-ACT-003** MANDATORY: Ensure build and transformation routines read configuration and schema files synchronously during initial execution and fail early with diagnostic messaging when prerequisites are missing.

### Verify

```bash
# Discover the workspace validation task from repository configuration and run the script linter
# Search the script directory to confirm all core module imports use the explicit protocol prefix
```

**Accept when:**
- All automation script files importing core runtime capabilities use the explicit protocol namespace without bare specifiers.
- The discovered repository script validation and linting suite completes with zero errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>