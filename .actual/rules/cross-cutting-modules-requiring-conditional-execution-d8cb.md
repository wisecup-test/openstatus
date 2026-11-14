---
version: 2
name: cross-cutting-modules-requiring-conditional-execution-d8cb
description: "@t3-oss/env-core Runtime Environment Schema Validation: Modules Requiring Conditional Execution Flags Runtime"
metadata:
  actual:
    adr_id: d8cb3acb-b589-40ba-abea-d1fda2940e41
    status: accepted
    candidate_group_id: cad1aa2b-c9bb-4478-868e-db3d8e75bdc9
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Runtime Environment Schema Validation: Modules Requiring Conditional Execution Flags Runtime

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-T3-001** SHOULD: Modules requiring conditional execution flags or runtime overrides SHOULD model them as validated boolean or enumeration fields within the centralized schema rather than inspecting unparsed strings.

### Verify

```bash
# Discover and execute the project configuration validation test suite to confirm schema compliance across all declared environment variables.
# Discover and run the repository static analysis linter to ensure zero ambient process environment queries exist in service router files.
# Execute the project test harness with missing optional and required configurations to verify anticipated failure boundaries and fallback behaviors.
```

**Accept when:**
- All environment variable reads in service routers and runtime handlers reference the centralized schema export rather than global process lookups.
- Static configuration analysis and repository validation scripts pass without detecting unvalidated ambient environment queries.
- Missing required environment variables trigger immediate startup validation errors with descriptive diagnostic messages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>