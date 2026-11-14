---
version: 2
name: cross-cutting-runtime-environment-variable-access-rout-bb5e
description: "@t3-oss/env-core Runtime Environment Schema Validation: Runtime Environment Variable Access Route Through"
metadata:
  actual:
    adr_id: bb5e4633-2fb8-4d16-8880-7e263bc128ee
    status: accepted
    candidate_group_id: cad1aa2b-c9bb-4478-868e-db3d8e75bdc9
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Runtime Environment Schema Validation: Runtime Environment Variable Access Route Through

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all server-side API routers, service adapters, background procedures, and integration clients requiring runtime configuration parameters, and all environment variables defining credentials, webhooks, region indicators, and third-party API keys.

### Rules

- **R-ENV-001** MUST: All runtime environment variable access MUST route through a centralized configuration schema defined and validated with `@t3-oss/env-core`.

### Verify

```bash
# Discover and execute the project configuration validation test suite to confirm schema compliance
npm test -- --grep "env"

# Run the repository static analysis linter to ensure zero ambient process environment queries exist in service router files
npm run lint
```

**Accept when:**
- All environment variable reads in service routers and runtime handlers reference the centralized schema export rather than global process lookups.
- Static configuration analysis and repository validation scripts pass without detecting unvalidated ambient environment queries.
- Missing required environment variables trigger immediate startup validation errors with descriptive diagnostic messages.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests introducing raw environment variable access without schema mapping are blocked from merging.
</enforcement>