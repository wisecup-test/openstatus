---
version: 2
name: cross-cutting-before-implementing-changes-configuratio-5154
description: "@t3-oss/env-core Runtime Environment Schema Validation: Before Implementing Changes Configuration Modules That"
metadata:
  actual:
    adr_id: 5154b284-4e2d-49f3-8d1f-57d39e093a98
    status: accepted
    candidate_group_id: cad1aa2b-c9bb-4478-868e-db3d8e75bdc9
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Runtime Environment Schema Validation: Before Implementing Changes Configuration Modules That

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all server-side API routers, service adapters, background procedures, integration clients, and environment variable configurations.

### Rules

- **R-T3ENV-001** MUST: Before implementing changes to configuration modules that consume `@t3-oss/env-core`, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency version and verify API compatibility against official reference documentation.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory via automated static analysis, peer code review, and runtime initialization test suites.
</enforcement>