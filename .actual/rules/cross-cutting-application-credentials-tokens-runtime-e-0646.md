---
version: 2
name: cross-cutting-application-credentials-tokens-runtime-e-0646
description: "@t3-oss/env-core Secret and Environment Variable Validation: Application Credentials Tokens Runtime Environment Variables"
metadata:
  actual:
    adr_id: 064635fc-2592-41a5-81aa-43ba024e7588
    status: accepted
    candidate_group_id: a46b086e-afb5-4b0c-9937-cd3c83432804
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @t3-oss/env-core Secret and Environment Variable Validation: Application Credentials Tokens Runtime Environment Variables

Scope: applies to `packages/api/src/**`.

These rules are ALWAYS ACTIVE for all API routers, background workers, and service adapters accessing runtime secrets, API keys, or operational environment configuration.

### Rules

- **R-ENV-001** MUST: All application credentials, tokens, and runtime environment variables MUST be defined and parsed using the adopted @t3-oss/env-core schema specification.
- **R-ENV-002** MUST (DISCOVERY POLICY): Discover all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-ENV-003** MUST (LOCK-VERSION GROUNDING): Before writing code using a versioned library, find the dependency manifest, identify the build tool, inspect the repository lock/resolution artifact, look up official documentation for that exact version, confirm every API/class/function exists, and re-run per dependency at point of use for version-sensitive behavior.
- **R-ENV-004** MUST: Define all server secrets within the server schema property of the environment definition function.
- **R-ENV-005** MUST: Import the validated configuration object directly in consumer modules rather than reading raw process variables.

### Verify

```bash
# Discover the repository verification script from the project manifest and execute the test suite to validate schema parsing
# Execute the repository static analysis and linting script to verify absence of direct runtime environment reads outside the environment configuration module
```

**Accept when:**
- All server-side secrets and environment configuration variables are declared and validated through the centralized schema module
- Direct untyped access to runtime environment variables across application routers and service handlers is eliminated

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests containing unvalidated direct access to environment variables fail continuous integration checks and require remediation, and deployment pipelines reject application startup when mandated secret declarations fail schema validation constraints.
</enforcement>