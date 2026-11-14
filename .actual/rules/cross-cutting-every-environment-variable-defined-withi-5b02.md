---
version: 2
name: cross-cutting-every-environment-variable-defined-withi-5b02
description: "@t3-oss/env-core Runtime Environment Schema Validation: Every Environment Variable Defined Within Schema"
metadata:
  actual:
    adr_id: 5b028dcc-7244-42c0-ba52-76de7c14ac5d
    status: accepted
    candidate_group_id: cad1aa2b-c9bb-4478-868e-db3d8e75bdc9
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Runtime Environment Schema Validation: Every Environment Variable Defined Within Schema

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all server-side API routers, service adapters, background procedures, and integration clients requiring runtime configuration parameters, and all environment variables defining credentials, webhooks, region indicators, and third-party API keys.

### Rules

- **R-ENV-001** MUST: Every environment variable defined within the schema MUST declare an explicit validation parser and fail fast during initialization if required configuration values are missing or malformed.

### Verify

```bash
# Discover and execute the project configuration validation test suite
npm test -- --grep "env"
# Run repository static analysis linter to ensure zero ambient process environment queries exist in service router files
npm run lint
```

**Accept when:**
- All environment variable reads in service routers and runtime handlers reference the centralized schema export rather than global process lookups.
- Static configuration analysis and repository validation scripts pass without detecting unvalidated ambient environment queries.
- Missing required environment variables trigger immediate startup validation errors with descriptive diagnostic messages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification via automated static analysis and test execution is mandatory.
</enforcement>