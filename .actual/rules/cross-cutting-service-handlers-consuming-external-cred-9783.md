---
version: 2
name: cross-cutting-service-handlers-consuming-external-cred-9783
description: "@t3-oss/env-core Secret and Environment Variable Validation: Service Handlers Consuming External Credentials Access"
metadata:
  actual:
    adr_id: 97833fd9-3fcf-4bdd-9496-d74117adfaac
    status: accepted
    candidate_group_id: a46b086e-afb5-4b0c-9937-cd3c83432804
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Secret and Environment Variable Validation: Service Handlers Consuming External Credentials Access

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all API routers, background workers, and service adapters accessing runtime secrets, API keys, or operational environment configuration.

### Rules

- **R-ENV-001** SHOULD: Service handlers consuming external credentials SHOULD access them via typed object properties exported from the validated environment module.

### Verify

```bash
# Discover and execute the project test suite to validate schema parsing
# Discover and execute the repository static analysis and linting script to verify absence of direct runtime environment reads outside the environment configuration module
```

**Accept when:**
- All server-side secrets and environment configuration variables are declared and validated through the centralized schema module
- Direct untyped access to runtime environment variables across application routers and service handlers is eliminated

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis and lint checks scanning for raw environment variable reads outside the dedicated environment schema module must pass.
</enforcement>